# kubectl - basics
In the last lesson we created a local cluster; but now we want to interact with it. We want to do things with it. The main tool we are going to be using is kubectl. 

There are many tools that you can use, some bringing a graphical interface and others just give you a different way of interacting with clusters or bring the ability to interact with multiple clusters. But as kubectl is the tool used for the kubernetes exams as well as the base for all of these other tools, I'll be sticking to it as the CKA and CKAD exams really test your ability to quickly use the kubectl command line. But not only that it is probably the most prevelent tool out there.

Ok, so we installed kubectl in the last lesson, if you missed that go back to it and it will help you get that installed.

Instead of starting with a few commands that do things. I wanted to start by breaking down the dycotomy of the tool so that going forward you can find your way around it and we are going to cover some of the most basic commands.

The very first thing you need to know is that kubectl uses a file called the kubeconfig to access your cluster. By default if you are using kind or minikube it will create and put this file in your `~/.kube/config` and kubectl will use this file by default.

This file can acctually have more then one cluster in it but that is byond the scope of this lesson. You can also specify where this file is if it's not in this default location by using the `--kubeconfig` global flag with any and all kubectl commands.

Ok, so lets talk about the 10 most important commands to know about. And then we will talk about some you might have expected to be on this list, and are super useful in the right way, but that way might not be exactly how you expected to use them.

`get` get and print out the current configuration of a resource, this could be a pod, deployment or even your own custom resource
`expose` - lets you expose a replication controller, service, deployment or pod as a new service
`apply` - apply a resource, this can create and update resource based on these files
`delete` - delete a resource
`create` - create a resource - mostly used to create templates of files that can later be applied
`edit` - updated a currently deployed resource
`proxy` - lets you make calls on your local host like it was running inside the kubernetes cluster, this can be useful for debuging as well as learning and extreamly advanced kubernetes api calls.
`dexcribe` - describes the resource in question giving you information on it's current state
`explain` - built in documenation of the different kubernetes resources and is really powerful when learning
`logs` - returns the logs of a container in a pod, remember there can be more than one container
`debug` - creates a debugging session enabling quick debugging of missbahving pods.

Now there are a lot of other commands that you can explore and a lot of options for each of these commands

Lets talk about each one of these in a little detail.

`kubectl get [resource]` this can be used to get the current resource in a cluster and understanding how that object is stored in kubernetes. my favorite flag to use with this command is the `-o` to change the output. for example, run `kubectl get nodes` and `kubectl get nodes -o wide` and see all the other usefil data. But it does not stop here, you can have it print out in a machine readable format like json or yaml but you can also use jsonpath here. Play around with it and remember it and the different options to better understand the desired state of your resources.

`kubectl expose [resource]` While we are going to go into more depth on services and the likes in the future, just know now these is a way to do it viea the command line. For the most this is not that useful exept when playing around with your cluster or using the --dry-run=client to generate yaml for you. What a lot of people don't know or relize is that the kubectl command line can create all of these resource files for you, so if you get overwhelmed trying to remember what goes in what type of file, kubectl can usually create the base template for you not only speeding up your work but also ensureing you waste less time on miss typed configuration templates..

`kubectl apply [-f or -k]` this will let you apply a file or kustomize directory, we are not going to dig into kustomize in this class but it's a very powerful tool for managing configuration. Apply, unlike create or edit can do it all. This is a little more advanced, but it's also much more useful to actually manage your cluster with and lets you make updates based of changes in files that can be stored in a system like git.

`kubectl create [resource]` while you can create resources in kubernetes this way I don't think that is the most useful use of it. kubectl create's hidden super power is the ability to create base yaml files that you can then update and apply to your cluster. These yaml files are hard sometimes to remember exactly what is in them and how they are formatted. with `kubectl create [resource] name --dry-run=client -o yaml > my-resource.yaml` will create a base resource for you filled out, you can then update the file with anything special and use `kubectl apply -f my-resource.yaml` this not only lets you check the file easilly into a repository, but if later you make changes to it and want to update your clauster all you need to do is run `kubectl apply -f my-resource.yaml` again and your cluster will now have the latest configuration.

`kubectl delete [resource]` is my favorite way to delete things. that is about all the extra information I know to add here.

`kubectl edit [resource] [name]` is going to let you edit a current resource. I would say use this with care and more of a developer tool, most of the time things in production should be managed by other tools but this is really useful when you are dealing with development clusters and experimenting with configuration.

`kubectl proxy` is going to let you make calls like you are inside the cluster, this has a lot of uses for debuging as well as learning as you can directly interact with the kubectl API without worrying about dealing with authenticating with the kubeconfig file. Not only this but it lets you debug services and things like that that are only accessable inside the cluster!

`kubectl describe [resource] [name]` lets you descrive a resource and is another tool for debugging the current status of objects and is one of the first places I go if I see something amis inside a cluster or deployment. 

`kubectl explain [resource]` is built in documentation. But really really good documentation. The best part is it's from your cluster! meaning it is documentation on the resource for your version of the resource. not only it lets you dig around in it but is documentation right at your finger tips meaning if you run into something and use describe you don't have to go searching on the internet for it. So if you want information on the `pods` manifest file you can run `kubectl explain pods` and get information on it's manifests and is a excelent tool when taking exams if you forget what manifests contain what properties!

`kubectl logs pod` lets you get logs from a pod. mind you, if your pod has more then a single container, you need to specify what containers you want the logs from with the `-c`

`kubectl debug` is a really awesome way to debug a container but is deffenetly a more advanced topic. I only mention it here because as soon as you have famillierarity with kubectl and kubernetes you should dig into it as it will expaind your debugging abilities.

