# Lesson 14: Cluster Networking

The goal of this lesson is to introduce you to concepts and techniques used for networking in your kubernetes cluster. We can already start to understand the issue and I just wanted to lay it out a bit clearer for people so that hopefully we have a good understanding of what I mean when I say, the cluster network, the node network and kubernetes services. These all use the concepts built in kubernetes networking and all hold to some values. But you have ot understand how they work together to build effective kubernetes clusters.

Lets start simple, Kubernetes builds a network of pods, on a network of nodes. so we obviously have two different networks. the one is more of the physical (even though these machines could be VM's) and the other network is more of a virtual network inside kubernetes. But there is even a third network, and this is the one that exists inside a pod that lets containers talk to other containers on localhost! I'm bringing this up just so we remember it! The really important thing to understand is that these are different networks that exist on the same machines, so all the pods, and the pods' network needs to be distinct and separate from the nodes network.

Now kubernetes made some ground roles.

Pods on a node can communicate with all pods on all nodes without a NAT.
Agents (think kubelet) can communication with all pods on that node
Pods on the host network can communicate with all pods on all nodes without NAT

And I would go on to say "all nodes have to reach and talk to all nodes with out NAT" though this one is not strictly required it is difficult to get around and requires making kubernetes think all the nodes are on the same network!

to do this kubernetes created something called the Kubernetes networking model, or the CNI. oh wait. it's confusing. it's the Container Networking Interface that lets you write your own plugin to handle networking in the kubernetes networking model! This is really cool because this lets everyone handle networking just how they want with just the requirements they want! But you probably don't want to write your own! don't worry, there are a TON of projects out there already. My personal favorites are Calico and Cilium. Calico is easier to use, Cilium seems to be a little more bleeding edge with some really cool networking stuff using ebpf.

Now, while this is really cool. understand that most distributions come with their own networking solution so most of the time you don't have to worry about it! Why am I talking about it? because you are taking a class provided by the null channel. We don't leave many rocks unturned if we can help it!

Ok. so just remember, you have three networks, the node network, the pod network and the pod's network! And yes, I have argued with many people that the `CNI` should be called the `PNI` as it's really the pod network interface, but for some reason people don't listen too me.

Want to dig into more? don't worry, in some of the more advanced classes we are going to even look into how to implement our own CNI and use it in a kubeadm made cluster. That being said it's not something you need to worry yourself with right now. Just understand that whenever a new pods is stood up the CNI is what is responsible for giving it an IP address. and further more you put limits on how many pods you can have based on the network cider you pick for it!

Understanding the CNI
Discovery of some of the different features of different CNI’s 
How to install one using kubeadm
