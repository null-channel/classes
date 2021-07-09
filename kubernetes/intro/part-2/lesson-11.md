Lesson 11: Kubernetes Services - cnt.

Now the third type of service you can create is a LoadBalancer. We are going to cover them a bit here, talk about how they work but then in the end, we are not going to do anything with them as they only really work in the cloud. Now that being said, if you do want to use load balancers and are one that is not worried about taking the path less traveled go ahead and look up a tech called metalLB it's your friend and can provide this same experience on bare metal/vm's.

Ok. so what does a service of type loadbalancer mean? well it's a type that is provided by your cloud provider and it sticks a loadbalancer in front of your service. this is interesting as really all this does is provide you a way to provision a load balancer in your cloud that knows where a nodeport kubernetes service is. That is not quite how most or implemented but understand that this IS implemented by your cloud provider and not kubernetes itself and each one implements it a little differently to work with thier clouds. I'm going to show off a few loadbalancers in a few clouds here so you all can see how they work, just a few words of caution:

1) Loadbalancers will expose this service publicly, or at least by default it will. So be careful!
1) They cost money! most if not all clouds charge for their loadbalancer services so making a bunch of them might not be a good idea
1) The use of a LB might be best put to use with a technology called ingress, giving you the best of both worlds while minimizing the number of Load Balancers you need to create.

# TODO: this needs more something something. it's maybe a little two short with not enough education

Ok. so that is a service of type Load Balancer, they are pretty cool. Lets look over the output of a cluster with an active LoadBalancer.

As you can see here, unlike nodeport and clusterIP the LB services get a "external ip" address that we can use to access our application! This is really cool and super useful in a lot of ways. Lets check this out on a few different clouds!


```
Show:
AWS LB
GCP LB
Azure LB
Digital Ocean LB
```
