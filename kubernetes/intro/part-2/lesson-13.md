# Lesson 13: Rolling updates and Rollbacks

We have talked a little about how you can update your deployments, Lets make a new deployment of nginx, but this time we are going to do something a little more interesting with it.

run 
`kubectl create deployment nginx --image=nginx:v1.19` We can then go ahead and scale this out with 3 replicas with
`kubectl scale deployment nginx --replicas=3`

Ok. so we have a deployment, but now we really want to update to `nginx:v1.20` how might we do this?

I want to first quickly go over what is happening in kubernetes becuase you might remember I talk about how deployments did not do it all alone, they get help from something called replica sets. In fact if you look at this "replicas" flag we are setting this is a feild that is passed to the replica set. That is right, it's job is to make sure the right number of pods from the spec are being run. And it's a bit more powerful then that as you can set the replicaset as a horizontal pod autoscaler target. That is pretty cool. But right now lets focus on updating the image and rolling out a new update!

Now it's important to understand here that if this deployment sits behind a service the service will handle routing traffic only to pods that are active so you don't have to worry about it. What a deployment does, and why you should probably not use a replica set directly is it lets you rollout changes, new state, to you pods. How does it do it? well lets do it first, and then I'll show you some pretty annimations that hopefully will describe it in enough detail while not being boring.

There are three main ways you can do this (there are others with other tools but... we are sticking to kubectl)
`kubectl set image deployment nginx nginx=nginx:1.20.0 --record`

`kubectl edit deployment nginx` and change the `image` felid on the container spec, saving and returning.

and lastly, if you made this in a file, you can update the yaml file and run
`kubectl apply -f name-of-file.yaml`

well cool! if you are fast enough you can even check on the status of this deployment! run
`kubectl rollout status deployment nginx`

So lets explain what happens behind the curtin. The very first deployment starts with a single replica set. and then we scaled it up to 3 pods. and that replica set created three pods. But then we edited the deployment. the pod spec. It means that we need all new pods. Instead of having the replica set capable of running two versions of the app, it can't so what a deployment does is brings up a new second replica set and deploys it. It will first deploy it with a single pod, once its up, it will bring one down in the first replica set and then scale up the second replica set to have two. and it will keep doing this until your deployment is at the requested replicas!

Well cool. Thats not too bad after all! and this is how replica sets and deployments work together! Dont belive me? run `kubectl get rs` or `replicasets` and you will now see 2 replica sets for the `nignx` deployment! one of the replicasets has a desired state of 0 pods!

But there is more! deployments also give us the ability to roll back. I'm sure we have all been there when we made a deployment and though it was awesome but in the end it turned out to rash because of a typo or something! well, kubectl lets us roll back quickly and easilly.

First lets check the history.

`kubectl rollout history deployment nginx` is going to list the revisions of the deployment so far.

you can see here we have 2. want more information on a single revision?

`kubectl rollout history deployment nginx --revision=1` and this will tell you everything you want to know about that version

Ok, but you want to go back, and you want to go back RIGHT NOW. so lets do it. Here again there are two ways of doing it. The first way

`kubectl rollout undo deployment nginx` will take you back exactly one revision of the app. Alternatively you can run 

`kubectl rollout undo deployment nginx --to-revision=1` if you want to go to a specific revision!

and this will take us back to the original "working" form of out app! Now there is so much more that we can get into. but this is your basic updates and rollbacks! And don't worry, if you thought this was just too much fun and not technical enough, don't worry, networking is next!

