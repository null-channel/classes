# Why pods are not the answer

We just spent a LOT of time with pods. but as I started the last lesson off with, most of the time, we don't deal with bare pods. So what do we use? Deployments. Think of kubernetes objects that can control other kubernetes objects and layer functionallity. Almost like the unix principle of "do one thing and do it well" each of these "things" are able to focus on one thing. So a pod's focus is on the containers, the resources and the general running of a logical set of processes. So it makes since that there woudl be something that is worried about running pods!

## Deployments

Lets talk about Deployments. Now I say deployments but deployments don't do it all by themselves! They get some help. But lets first talk a little at a high level what a deployment is, what problem it solves and then we will get into using one. Once we have used one, we are going to dig into how it works and see who is helping it out!

Lets start this lesson out by first creating a deployment and learning from using.

`kubectl create deployment nginx --image=nginx --dry-run=client -o yaml > nginx-deployment.yaml`

This, like the `run` command for pods will create a deployment. Now all you have to do is run `kubect apply -f nginx-deployment.yaml` Or if you did not want the local file you could have left off the `--dry-run=client` and the `> nginx-deployment` part and it would have created it directly on your kubernetes cluster.

Ok. so now this deployment is running in our cluster. Lets check it out!

So we can use `kubectl get deployments` to list all our deployments in the default namespace, of witch we only have a single deployment as of yet. Lets get that deployment. `kubectl get deployment nginx` will return the new deployment and give some information. Now this is not a lot of information and I hardly ever run a `kubect` command without using the `-o wide` option. But there are a lot of output options, From `wide` to `json, yaml and jasonpath` giving you all the options you need for obtaining just the information you want or all of it.

If you noticed the replicas was set at one. Hmmmm. lets verify how many are running. we can use `kubectl get pods` and you can see only one is listed. We probably want 3 running just to make sure if one crashes we have another and to handle any increase in load. Lets do that now.

`kubectl scale --replicas=3 deployment/nginx` and now if you run `get pods` you can see more pods getting created in the default namespace and we did not have to create or manage any pods by hand, In fact. lets put this to the test, we are going to simulate a failure. We are going to remove a pod that is managed by a deployment. `kubectl delete pod [pod-name]` is all you have to run. so use `kubectl get pods` first to find a name and then go ahead and delete it! so. now we should have 2 pods running right? wait a minute? the deployment has already fixed the issue and we now have 3 pods running still! This is a super useful tool!

Now, We scaled this by using it's `[type]/[name]` or `deployment/nginx` but you can do this using it's file `-f deployment-nginx`. On top of this scale can be used on pretty much anything that has a replication factor, so think replication controllers or replica sets!

Lets take a quick look over the yaml file that was created for us

```
apiVersion: apps/v1
kind: Deployment
metadata:
  creationTimestamp: null
  labels:
    app: nginx
  name: nginx
spec:
  replicas: 1
  selector:
    matchLabels:
      app: nginx
  strategy: {}
  template:
    metadata:
      creationTimestamp: null
      labels:
        app: nginx
    spec:
      containers:
      - image: nginx
        name: nginx
        resources: {}
status: {}
```

You can see here, we could also just update the number of replicas here and run `kubectl apply -f deployment-nginx.yaml` again to update it! Super cool. Ok. moving on you will notice this has a few `spec` fields, not just one. The first `spec` is the spec for the deployment and the second nested one is the `spec` for the pod, you could add another container here or update this to be a new version of nginx!

You will also see some things around strategy, labels and some other things, these are important, but we will cover those at a later time, other then the label, we will talk about that now. 

Every object in kubernetes can be labeled, that being said lots of things use these labels. And we are going to explore one way those are used right now. Know how I said that deployments did not do it alone, they had help? Enter The ReplicaSets. Lets see them. you can run `kubectl get replicasets` or `kubectl get rs` and see the replica set being used for our nginx. Now if we had a lot of deployments and a lot of replicasets, this could be hard to know what one is what and that is where the labels come in. You can use `kubectl get replicasets -l app=nginx` this is just one of the ways that labels can be super helpful! But trust me, thier use is even greater then this as you will see in the future!

Ok. I don't want to get to in depth about replicasets but lets just cover it at a high level. You can use Replicasets by themselves but if you use a deployment it owns the replicaset and is the main way of using replicasets. Deployments use them to orchestrate pod creation deletion and updates. This is really cool, so at this point we have deployments using replicasets using pods using containers! Deployments are the main way to deploy your app and most other ways would be considered an "advanced" solution. This is awesome as we now not only know how deployments work at a high level but we also how they work and how they run containers in the end. In the future we will understand more of why each of these in the deployment stack is there but for the most part, each one is worried about a single issue and does that thing well. Pods worry about containers, replicasets manage the number of desired pods and lets you roll out new specs to those pods and deployments bring it all together with declarative updates for the pods and replicasets enabling you to describe a desired state.

Now, there is a lot more to learn here, but we are going to move on so we can understand more about the ecosystem around deployments to help you manage the deployment of your app!