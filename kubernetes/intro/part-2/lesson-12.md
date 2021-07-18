# Lesson 12: Ingress 

Ok, so services expose, well... services. Right. But most likely if you are looking at kubernets you have a whole fleet of services that all make up your application or infrastructure. This means you are going to probably want to interact with them seamlessly, if we used a loadbalancer for each one not only would our cloud costs sky rocket but we would have a different IP address/domain for each of our services. Now you might want this for some strange reason, like not liking money. But for the most part we are not wanting to have a public IP address for each service. Think of your web app, it might have an accounts service, it might have a live feed service, a service to aggregate logs and a service to provide users with a UI or whatever else your application needs. You might even want to easily and simply get versions of your different api's so that you can easily migrate traffic to newer versions of services while supporting the older versions for a limited amount of time. Thankfully kubernetes gives us a way to do this.

Before we dive into this, I want to be clear, Ingress does more then just give you a way to rout traffic into your service but is generally used for http traffic, custom load balancing, TLS termination as well as name based virtual hosting. While we might get into some of these more complicated later in more advanced settings but I thought it was good to bring up as TLS termination and the likes are very popular uses for ingress as well. But for now, we are going to keep it simple. Very simple.

The object we are going to talk about is Ingress, now ingress works in conjunction with an ingress controller, something we will install in our a little bit but lets explore what kubernetes ingress resources are first, then we are going to make a deployment with a few services and see ingress at work.

Ok. so here is what ingress looks like at a high level. To explain how it works simply you have a ingress managed load balancer that sits outside your cluster, that then sends it to your ingress that follows routing rules to map that request to the proper request. It's important to understand here that ingress is designed for port 80 and 443, or more importantly it's designed for HTTP and HTTPS, if you are doing something special with a protocol that is not HTTP/HTTPS then ingress is not for you and you can just use LoadBlancers or Nodeports directly.

Something to understand before we jump in is that you can actually have multiple ingressClasses that can use different configuration and controllers, this is actually really important because you can have different ones responsible for different things. Like TLS termination.

Ok. So lets install an ingress controller, its important to understand that if you don't install an ingress controller, ingress can't do anything. you can create the resource all you want but it will not do anything!!!

So i'm going to quickly explain how to install ingress on Kind, But you will find the links for kind and minikube with this video so you can have the most up to date information when you do it.

Minikube: https://kubernetes.io/docs/tasks/access-application-cluster/ingress-minikube/
Kind: https://kind.sigs.k8s.io/docs/user/ingress/

So, if these instructions don't work for you, try and see if anything has changed. If you are running a cluster in the cloud, your ingress controller should have instructions for installing on your cloud.

ok. so if you are using kind, we are going to using the nginx ingress controller, there are others that work with kind I just think this is one of the more popular right now. To install an ingress controller on kind you have to create a new cluster with a configuration file.

```yaml
kind: Cluster
apiVersion: kind.x-k8s.io/v1alpha4
nodes:
- role: control-plane
  kubeadmConfigPatches:
  - |
    kind: InitConfiguration
    nodeRegistration:
      kubeletExtraArgs:
        node-labels: "ingress-ready=true"
  extraPortMappings:
  - containerPort: 80
    hostPort: 80
    protocol: TCP
  - containerPort: 443
    hostPort: 443
    protocol: TCP
```

To create a cluster with this file use 
`kind create cluster --config=/path/to/config`

Once the cluster is up and going run:
```bash
VERSION=$(curl https://raw.githubusercontent.com/kubernetes/ingress-nginx/master/stable.txt)
kubectl apply -f https://raw.githubusercontent.com/kubernetes/ingress-nginx/${VERSION}/deploy/static/provider/kind/deploy.yaml
```

you can wait for it to be ready using 
```bash
kubectl wait --namespace ingress-nginx \
  --for=condition=ready pod \
  --selector=app.kubernetes.io/component=controller \
  --timeout=90s
```

Ok, so now that we have a cluster set up with an ingress controller, we need a deployment and service to really test it out and explore!

Go ahead and deploy our two services with this file
`kubectl apply -f ./lesson-12-deployment.yaml`

If you look though this file it's pretty simple, it has two deployments, one that echo's foo and the other that echos bar. Each of these deployments have a service that exposes them, notice how these services do not have a type? do you remember what the default type of service is? Thats right, ClusterIP. Meaning these don't even have a out of cluster IP!

Ok. lets create a ingress object for this! now remember this is just a really simple example, there is so much more you can do with these!

```yaml
apiVersion: networking.k8s.io/v1
kind: Ingress
metadata:
  name: example-ingress
spec:
  rules:
  - http:
      paths:
      - pathType: Prefix
        path: "/foo"
        backend:
          service:
            name: foo-service
            port:
              number: 5678
      - pathType: Prefix
        path: "/bar"
        backend:
          service:
            name: bar-service
            port:
              number: 5678
```

Lets break down this yaml file. First our version, you can see that ingress is in the networking.k8s.io/v1 version. meaning this is a stable api in the kubernetes networking namespace. cool. our kind is Ingress, and we have named it `example-ingress`

So lets check out the spec. Really we just have two rules and are pretty self explainitory. The pathtype can be one of three options, `ImplementationSpecific` meaning its up to the IngressClass, `Exact` meaning it must match exactly and is Case Sensitive and `Prefix` meaning it matches based on a URL prefix split by `/` Now this is where it gets a little fun. multiple types between exact and prefix can match! In these cases first the longest match will take precident, then if they are of equal length exact takes precedence over prefix.

Ok. so apply this file with `kubectl apply -f ./lesson-12-ingress.yaml` and test it out!!

```bash
# should output "foo"
curl localhost/foo
# should output "bar"
curl localhost/bar
```

Super cool. we have learned to use ingress to map different HTTP based requests to different applications/services! In the next lesson we will learn how to roll out and roll back deployments!