Lesson 10: Kubernetes Services

So we have created a deployment back in lesson 7. But how do we expose this outside the cluster? or even inside the cluster to another service? One of the wonderful things about a deployment is that we don't have to worry about where a pod is running, on what machine, or even what it's IP address is. But if we don't know what it's IP address is, how do we access it? This is where the kubernetes resource called "services" comes in.

There are four types of services you can create, 

```
ClusterIP
NodePort
LoadBalancer
ExternalName
```

Now, I don't want you to worry about all of these today, First we are just going to focus on ClusterIP and Nodeport. In the next lesson we will cover loadbalancers and you will learn more about them then you knew you wanted too, including just a little of how they work at a high level and what sets them apart from other types of cluster services.

So. we have an nginx deployment, its running in a pod, or pods somewhere, on some machine with some IP address and we want to access it. So we will create a service for it. You know how I talked about labels being important. Well here you will see how services can expose deployments using labels!

ok. lets create our first service, now by default, unless stated otherwise a service will be created as a `ClusterIP` A clusterIP services is going to expose this inside your kubernetes cluster, but nothing outside the cluster will be able to reach it. This could be used to expose internal resources, like a database or an api! They are actually quite a bit more useful then you might think.

Now I know you have been using the `kubectl create` command but there is a different one for creating services! While I might argue it would make since to have it under "create" it's quite easy, and fitting where it is. `kubectl expose` is going to be what we use, but other then swapping `expose` out for `create` much is the same but there are a few things to note!

ok. So the command we will use to expose our deployment is `kubectl expose deployment nginx --port=80 --target-port=8000 --dry-run=client -o yaml > service.yaml` you will notice that we are telling it that it is a `deployment` and the name of the deployment we want to expose. Other then that we are just letting it know what ports inside the pod/container we want exposed and what port to map it to on the service! Now lets open up this yaml and see a little more what a service looks like!

```
apiVersion: v1
kind: Service
metadata:
  creationTimestamp: null
  labels:
    app: nginx
  name: nginx
spec:
  ports:
  - port: 80
    protocol: TCP
    targetPort: 8000
  selector:
    app: nginx
status:
  loadBalancer: {}
```

Ok. so the one interesting thing I wanted to point out here is the `selector` section. Everything else here is pretty standard. We have our ports, our protocol and that is pretty much it. We also have our name and a label for the service with it's name. But lets get back to this selector. You are going to find these use all over the place and they are really really cool. So using this selector lets us tell the service what pods we are wanting this service to serve. There are tons of advanced things you can do with this but we are going to keep it simple.

We can do the exact same thing that the service is doing, lets see it in action.

`kubectl get pods --selector=app=nginx`

This is going to list all the pods in the nginx app that we deployed. This is effectively the same thing the service is doing and how the service works to know what pods its routing traffic too. ok. So this made a ClusterIP meaning we can access this service from inside the cluster, but now outside. Well that means that there are two ways right now for us to hit this, We can ether use `kubectl proxy` or we can exec into a pod running on the cluster! 

Ok. so lets try both of these to test our new clusterip service.

So the first way is to proxy all or our requests into the cluster. This is useful for a lot of reasons but for now we are going to look at how to use it to test our service.

First you are going to run `kubectl proxy --port=8080 &` this simply maps all requests to port 8080, authenticates them and sends them to the kubernetes cluster. the `&` is there to run it in the background, you can also just run this in a second terminal if you so prefer.

if you run `curl http://localhost:8080/api/` while this is not about the kubernetes api, I suggest you check this out and get familiar with it. For today we are going to run `curl http://localhost:8080/api/v1/namespaces/default/services/nginx/` you can see here we are hitting the v1 namespaces, and our deployment is in the default namespace, Then we get the service `nginx` this is going to just return the json that describes the service! This is super cool. you can see exactly what the `kubectl get service nginx -o json` is! This is really awesome and super powerful. but we want to access the service. Just run `curl http://localhost:8080/api/v1/namespaces/default/services/nginx/proxy/` and now it will proxy the request!

now. this did not quite return what we wanted, lets fix it!
```
Error trying to reach service: 'dial tcp 10.244.0.5:8000: connect: connection refused
```

Well, that is because w emade the service proxy to port 8000!!! oops. well there are two ways we can fix this, ehter update our nginx deployment to serve on port 8000 or we can just update our service. lets do the later as updating out deployment would require us to configure nginx and we ahve not leanred out to best do that. 

Ok, so simply run `kubectl edit service nginx` and where you see `targetPort` change this to `80` Save your changes and they will automatically get applied! Cool. we just learned a new command in the `kubectl` tool. edit. This lets you edit live active objects in your kubernetes cluster. Now it's not a very good tool to use live in production and I would not use it there. But it's an awesome tool to use in learning and debugging! 

Ok. lets run our curl command again, there you have it. you can see our response from our nginx server! This is fantastic, Not only did we expose it, we tested and got a response using kubectl proxy. Now you might have remembered I said there was another way to access this clusterIP. That is though the exec command. If you are familiar with the docker command `exec` this will be almost the exact same.

To do this we are going to need to exec into a container, We could exec into nginx but for the sake of this lesson we are going to stand up a pod just for debugging.

`kubectl run debugapp --image=gcr.io/kubernetes-e2e-test-images/dnsutils:1.3 --restart=Never -- sleep 1d` don't worry about the strange image, it just has useful dns/networking utilities that are quite useful in these cases and we are going to use nslookup in the next step!

now you can run `kubectl exec -it debugapp -- sh` And this is going to drop you into a shell in that pod! Now you can run all types of commands. Or if you just have the one off and don't want to be thrown into an interactive terminal you can just run 
`kubectl exec -it debugapp -- nslookup nginx.default` Sweet. We can see our service is working and running! Not only this you have just learned quite a few ways to debug on the command line, another setp in learning that sweet sweet kubectl magic so many kubernetes expert wave around.

but enough about this. You might be asking, well I wanted to access this outside my cluster! and don't worry, there are a few options to do this, we will be learning one way this lesson and another next. The first way is using a service of type nodeport. remember above I said we where using a clusterIP and how it's the default? lets update our nginx cluster IP service to a nodeport service.

This is really not that hard, we are going to use the `kubectl edit service nginx` to edit the service and change the "type" from ClusterIP to `NodePort`and that is it. once you save it go ahead and edit it again and you will notice it has a new feild called `nodePort: [ip]` that IP can be accessed by any machine. Meaning that if you know the IP of any node in the kubernetes cluster you can access the service on [ip-machines]:[node-port] Now we are not going to play around with this right now because kind does not have a "machine" and you can pass a little bit of advanced configuration to fix this but is outside this lesson. But just understand that this is on ALL nodes in the cluster and works quite easilly.


Creating a NodePort service locally
