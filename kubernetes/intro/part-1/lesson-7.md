# Why pods are not the answer

So we just spent a lot of time with pods. but as I started the last lesson off with, most of the time, we don't deal with bare pods. So what do we use? Deployments. Think of kubernetes objects that can control other kubernetes objects and layer functionallity. Almost like the unix principle of "do one thing and do it well" each of these "things" are able to focus on one thing. So a pod's focus is on the containers, the resources and the general running of a logical set of processes. So it makes since that there woudl be something that is worried about running pods!

## Deployments

Lets talk about Deployments. Now I say deployments but deployments don't do it all by themselves! They get some help. But lets first talk a little at a high level what a deployment is, what problem it solves and then we will get into using one. Once we have used one, we are going to dig into how it works and see who is helping it out!

Lets start this lesson out by first creating a deployment and learning from using.

`kubectl create deployment nginx --image=nginx --dry-run=client -o yaml > nginx-deployment.yaml`

This, like the `run` command for pods will create a deployment. Now all you have to do is run `kubect apply -f nginx-deployment.yaml` Or if you did not want the local file you could have left off the `--dry-run=client` and the `> nginx-deployment` part and it would have created it directly on your kubernetes cluster.

Ok. so now this deployment is running in our cluster. Lets check it out!

So we can use `kubectl get deployments` to list all our deployments in the default namespace, of witch we only have a single deployment as of yet. Lets get that deployment. `kubectl get deployment nginx` will return the new deployment and give some information. Now this is not a lot of information and I hardly ever run a `kubect` command without using the `-o wide` option. But there are a lot of output options, From `wide` to `json, yaml and jasonpath` giving you all the options you need for obtianing just the information you want or all of it.

If you noticed the replicas was set at one. Hmmmm. We probably want 3 running just to make sure if one crashes we have another and to handle any increase in load. Lets do that now.