Lesson 13: Rolling updates and Rollbacks

We have talked a little about how you can update your deployments, Lets make a new deployment of nginx, but this time we are going to do something a little more interesting with it.

run 
`kubectl create deployment nginx --image=nginx:v1.19` We can then go ahead and scale this out with 3 replicas with
`kubectl scale deployment nginx --replicas=3`

Ok. so we have a deployment, but now we really want to update to `nginx:v1.20` how might we do this.

I want to first quickly go over what is happening in kubernetes and then 

Now it's important to understand here that if this deployment sits behind a service the service will handle routing traffic only to pods that are active as you can see here.

