# Why pods are not the answer

So we just spent a lot of time with pods. but as I started the last lesson off with, most of the time, we don't deal with bare pods. So what do we use? Deployments. Think of kubernetes objects that can control other kubernetes objects and layer functionallity. Almost like the unix principle of "do one thing and do it will" each of these "things" are able to focus on one thing. So a pod's focus is on the containers, the resources and the general running of a logical set of processes. So it makes since that there woudl be something that is worried about running pods!

## Deployments

Lets talk about Deployments. Now I say deployments but deployments don't do it all by themselves! They get some help. But lets first talk a little at a high level what a deployment is, what problem it solves and then we will get into using one. Once we have used one, we are going to dig into how it works and see who is helping it out!