# Pods

We ran a pod, and a deployment... Now lets understand a little more about them and what they are and what problems they solve.

## Pods - high level info

Pods are a collection of tightly coubled containers, I will start this by saying generally I don't think you should run multiple containers inside the same pod, but sometimes it can be useful and we will cover those in just a little bit. But first lets talk about what a pod is.

First to clarify, most people think of kubernetes as a container sceduler. But that is not quite true. While in the end it might be safe to say that it scedules containers but it's only indirectly though this concept of "the pod". A pod consists of one or many containers all networked together in such a way that they con communicate with the other containers in the pod like they where run on together on a VM, aka localhost. Mind you this means all containers share the same resources given to the pod. For example if you limit the memory and cpu usage of a pod, it's shared between all the containers that run. Not only is the network shared but you can also specify a subset of storage to be shared between the containers in the pod.

While we are going to look into creating a pod by it'self, there are many constructs that should be used instaed of a pod directly. Most notably the deployment and replicasets that will be discussed in futher detail later.

It's important to be famillier with pods as while you almost never want to run and manage them directly it's quite useful for debugging.

Lets dig into why I say you probably should not be running pods directly. Pods don't heal, scale or schedule. It will run on a Node until it is complete, or is stopped by a user or encounters an error. This pod will not rescedule, or repair itself.

Pods have five status, Pending, Running, Succeeded, Failed and Unkown.

Pending means that while the cluster has accepted it, its between scheduling and downloading images.
Running Succeeded and failed all are self explainitory.
Unknown means that kubernetes can't determain it's status.

## Pods - Doing the thing

Ok, so lets play around with some pods.

We are going to learn how to create, delete and describe a pod. If you remember that kubectl lets you do these commands exact commands for kubernetes resource, and a pod, is a kubernetes resource! So lets give it a try!

first lets check and see if there are any pods running.

`kubectl get pods` is all you have to run. Now we will learn about namespaces more but you will see that it does not find any pods in the default namespace. Cool, lets get a pod running.

Lets look at the `kubectl create --help` Interestingly enough you can see the available commands while lists a lot don't include pod. Now this does not mean we can't create a pod using create, we just need a pod spec file, Remember when I was talking about declarative? well all of kubernetes is based aroudn this idea. But we don't have a pod file to pass! lets run one. and then lets create the yaml file.

So first, kubectl has this great command called "run" it's mostly useless in production unless you are fixing a deployment in a bad state. and even then maybe it's not the way to go. But it's great when you are developing. Lets run this Pod!

`kubectl run nginx --image=nginx`

So, we are going to run a pod called nginx, and it's going to run the image `nginx:latest`

Now lets run `kubectl get pods` and you will notice we now have a pod running! But we still don't have the yaml file that represents this. 

We can ask kubernetes to describe it for us for a start
`kubectl describe pod nginx` will describe the pod resource nginx, Now this is not quite the way we want it. we want it in yaml. so lets run
`kubectl describe pod nginx -o yaml` awesome. But in the future we want to create these yaml files without deploying something to our cluster. lets do that right now.

`kubectl run nginx --image=nginx --dry-run=client > nginx-pod.yaml` this will make a file of what we deployed earlier wihtout actually deploying it! The best part, at this point you can edit this file and use it as a starting point and then deploy it using ether `kubectl create -f nginx-pod.yaml` or perferably `kubectl apply -f nginx-pod`!

Ok, enough fun with pods for now. lets delete the pod.
`kubectl delete pod nginx`

Ok. now up to this point we have only run a single container! lets fix that.

Run `kubectl apply -f https://github/null-channel/classes/kubernetes/intro/part-1/examples/multi-container.yaml`

How many containers does it have? well this is acctually quite easy to tell. just run `kubectl get pods` and here you can see this pod has three out of three pods running.

TODO:
Learn how to get logs from each container in a multi container pod (and single container pods)
Exec into a container in a multi container POD (and single container pods)
