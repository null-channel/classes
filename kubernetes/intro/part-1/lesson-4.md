# Local cluster

Ok, so to follow along we are going to do a lot of experimenting and I think its really important to get a local kubernetes cluster you can play around with. While I will try to provide as many labs and challanges online in your browser as I can, I think it's really important you have a plaground locally where you can try different things as we talk about them.

One technology that you can use is docker, docker desktop will enable to you run a local kubernetes cluster on both windows and mac directly from docker and if you are on those platforms is an excelent way to get a local kubernetes cluster. 

If you are on linux there are some other tools that provide an even better solutions as you can have multiple local clusters and tear down and stand up a new cluster even easier. These tools can be used inside windows WSL but requires a little extra work if you are going down that path.

Before we install a local cluster development tool, first we are going to need kubectl the main tool to interact with the kubernetes api. To do that you can follow the instructions found here https://kubernetes.io/docs/tasks/tools/ You not only get an up to date list on how to install kubectl, but it also has quickstart guides for kind and minikube all in one place! Most linux package managers package kubectl and then in that case it's just as easy as installing kubectl though it.

Some of the main local, development and testing cluster technology are:
minikube,
kind https://kind.sigs.k8s.io/
and
microk8s

So i'm going to drop links to these different technologies and you should find the latest install instructions. As these sometimes change i'm going to avoid putting them in the video but instead I thought maybe I would just go over some of my understandings of the projects and let you know what one I like using the most.

All three of the projects you just can't go wrong with, they all work and supply you with the tools needed. That being said if you are on a ubuntu based machine or already have `snap` installed `microk8s` is by far the easiest to install as it is just `sudo snap install microk8s --classic` and you have a cluster only a minute later. Now this is easy and it's powerful and there are a few times I defenetly use and recommend it, but for this class it's not my first recomendation as it does not let for as much experimenting and does not let you have multiple clusters running at the same time on the same PC.

minikube and kind are kind of a toss up. minikube is more powerful, kind is faster and easier. I find that I use kind about 75 percent of the time and just fall back on minikube when kind is not doing what I want or I want a little more control for some reason. Because of this I thought I would show you how to install and get your first cluster running with kind. That being said you should be able to follow along with any of the above solutions, when there is a divergance, like how to install a an ingress controller, I will provide documentation for each of the above technologies, while I will only demo one. Mostly I will stick to kind as I find it the easiest for people to get started with.

Ok. lets install kind, and create a cluster with it! Dont worry, it takes about 3 minutes total to not only install kind, but also stand up a local kubernetes cluster.

first ensure you have go 1.11 or greater installed, bassically you need a version of go with go module support. then all you need to do is run `GO111MODULE="on" go get sigs.k8s.io/kind@v0.11.1` and update that comand with the latest release of kind. If you have go's bin in your path you can now start using kind!

One thing I love about kind is it make creating a cluster just 3 words.

`kind create cluster` is all you need and it will even update your `kubeconfig` with the ability to access and use this cluster via the kubectl command line! To check that your cluster is up and running you can run the `kubectl get nodes` and you should see a single node with a status of ready! please be aware it can take a few minutes the first time you start up the cluster for the node to become ready!

Ok, And there we have it, a local kubernetes cluster ready to deploy all the pods we can think of! Thanks for joining me, in the next lesson we will cover how to use kubectl to start using kubernetes!
