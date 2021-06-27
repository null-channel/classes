Lesson 10: Kubernetes Services

So we have created a deployment back in lesson 7. But how do we expose this outside the cluster? or even inside the cluster? One of the wonderful things about a deployment is that we don't have to worry about where a pod is running, on what machine, or even what it's IP address is. But if we don't know what it's IP address is, how do we access it? This is where the kubernetes resource called "services" comes in.

There are four types of services you can create, but 

```
ClusterIP
NodePort
LoadBalancer
EnternalName
```

Now, I don't want you to worry about all of these, today we are just going to focus on ClusterIP and Nodeport. In the next lesson we will cover loadbalancers and you will learn more about them then you knew you wanted too.

So. we have an nginx deployment and we want to create a service for it. You know how I talked about labels being important. Well here you will see how services expose deployments using labels!

ok. lets create our first service, now by default, unless stated otherwise a service will be created as a `ClusterIP`


Creating a NodePort service locally
