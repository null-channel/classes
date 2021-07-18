# Pods

We ran a pod, and a deployment... Now lets understand a little more about them and what they are and what problems they solve.

## Pods - high level info

Pods are a collection of tightly coupled containers, I will start this by saying generally I don't think you should run multiple containers inside the same pod, but sometimes it can be useful and we will cover those in just a little bit. But first lets talk about what a pod is.

First to clarify, most people think of kubernetes as a container scheduler. But that is not quite true. While in the end it might be safe to say that it schedules containers but it's only indirectly though this concept of "the pod". A pod consists of one or many containers all networked together in such a way that they con communicate with the other containers in the pod like they where run on together on a VM, aka localhost. Mind you this means all containers share the same resources given to the pod. For example if you limit the memory and cpu usage of a pod, it's shared between all the containers that run. Not only is the network shared but you can also specify a subset of storage to be shared between the containers in the pod.

While we are going to look into creating a pod by it'self, there are many constructs that should be used instead of a pod directly. Most notably the deployment and replicasets that will be discussed in further detail later.

It's important to be familiar with pods as while you almost never want to run and manage them directly it's quite useful for debugging.

Lets dig into why I say you probably should not be running pods directly. Pods don't heal, scale or schedule. It will run on a Node until it is complete, or is stopped by a user or encounters an error. This pod will not rescedule, or repair itself.

Pods have five status, Pending, Running, Succeeded, Failed and Unknown.

Pending means that while the cluster has accepted it, its between scheduling and downloading images.
Running Succeeded and failed all are self explanatory.
Unknown means that kubernetes can't determine it's status.

## Pods - Doing the thing

Ok, so lets play around with some pods.

We are going to learn how to create, delete and describe a pod. If you remember that kubectl lets you do these commands exact commands for kubernetes resource, and a pod, is a kubernetes resource! So lets give it a try!

first lets check and see if there are any pods running.

`kubectl get pods` is all you have to run. Now we will learn about namespaces more but you will see that it does not find any pods in the default namespace. Cool, lets get a pod running.

Lets look at the `kubectl create --help` Interestingly enough you can see the available commands while lists a lot don't include pod. Now this does not mean we can't create a pod using create, we just need a pod spec file, Remember when I was talking about declarative? well all of kubernetes is based around this idea. But we don't have a pod file to pass! lets run one. and then lets create the yaml file.

So first, kubectl has this great command called "run" it's mostly useless in production unless you are fixing a deployment in a bad state. and even then maybe it's not the way to go. But it's great when you are developing. Lets run this Pod!

`kubectl run nginx --image=nginx`

So, we are going to run a pod called nginx, and it's going to run the image `nginx:latest`

Now lets run `kubectl get pods` and you will notice we now have a pod running! But we still don't have the yaml file that represents this. 

We can ask kubernetes to describe it for us for a start
`kubectl describe pod nginx` will describe the pod resource nginx, Now this is not quite the way we want it. we want it in yaml. so lets run
`kubectl describe pod nginx -o yaml` awesome. But in the future we want to create these yaml files without deploying something to our cluster. lets do that right now.

`kubectl run nginx --image=nginx --dry-run=client > nginx-pod.yaml` this will make a file of what we deployed earlier without actually deploying it! The best part, at this point you can edit this file and use it as a starting point and then deploy it using ether `kubectl create -f nginx-pod.yaml` or preferably `kubectl apply -f nginx-pod`!

Ok, enough fun with pods for now. lets delete the pod.
`kubectl delete pod nginx`

Ok. now up to this point we have only run a single container! lets fix that.

Run `kubectl apply -f https://github/null-channel/classes/kubernetes/intro/part-1/examples/multi-container.yaml`

How many containers does it have? well this is actually quite easy to tell. just run `kubectl get pods` and here in the ready column you can see this pod has three out of three pods running. You can also run the `describe` command as well as pass `-o yaml` or json to see more information on your running pods. But that is not the only information we could want to get about a given pod, We need logs!

Getting logs is easy, there is a command built right into kubectl. now. one might think that you type `kubeclt get logs` but one would be wrong in that thought. I still type it every once in a while. The logs command only works for pods, so we don't have to get logs for a pod. All we do is `kubectl logs [podid/name]` Now if you have been following along are are thinking "but there could be a few containers running in that pod, you would be right. If the pod only has one container as it should, you don't have to pass any arguments. But if it has multiple pods you have to pass the `-c` command with the name of the container you are wanting to get. This being said you can pass `--all-containers=true` and get all the logs for a pod.

You can check out all the flags you can pass here, but the most important ones to know about is the `-p` to get the logs of a previously terminated, `--tail` and `--since`. But i'll let you go look into that further!

One last thing, something that is super useful, you can exec into your pods/containers just like you do with the docker command line. Now I'm not going to cover all the flags or options but understand that this works mostly like you might think it would. And how would that be? well you might want to get "in" to your pod and poke around to see what is going on in this isolated container.

`kubectl exec POD -c [container]` now, there are two main ways to run this, ether a single off command with `--` or you can use the flags `-it` and get an interactive terminal inside your pod! Remember if you have multiple containers you need to pass the `-c` flag. otherwise you don't need too!

That is the fastest crash corse on pods, Dont worry if it's does not all make since just yet. It will. That being said, while we don't manage pods directly, it's the building block of everything we are going to be doing so it's important to understand well!

The next lesson we will learn some of the tech that is built up on top of the pod and enabled easy management of them!