# What is Kubernetes

What is kubernetes? If you are here you might already have an idea, but i’m going to start from the very beginning as I don’t know where you each are starting from, feel free to skip to wherever it becomes interesting for you!

So Lets start from the very start. 

Kubernetes is what a lot of people call a container orchestrator. Now, I would be tempted to argue this is incorrect on a technical level, but it’s an acceptable start. Now why would we want to orchestrate containers?

Well, as we know from our container lessons, containers brought us immutable infrastructure, or part of it at least. So containers give us the ability to repeatedly spin up the same process on any server we want. But now the issue we face is managing a bunch of processes running on a bunch of servers, and if one of those servers where to fail you need to reschedule that process, or container on another server. This is the core problem kubernetes looks to solve.

Kubernetes clusters a group of servers, and runs containers on those servers. Now, it provides other tools like deployments, replica sets and services, but all these tools are built around helping you manage these containers running on a wide range of servers.

So, kubernetes runs on a cluster of servers, It does this by running a control plane on a small group of servers, 3 to 7, that manages and maintains what is running and where. Now the rest of the machines in the cluster don’t run nearly as much as the control plane and mostly run just a client called the kubelet, and a container runtime, kubeproxy and everything else that will run on these machines will be containers!

This way, if one of these machines goes down, or if one of the container crashes for some reason, kubernetes will work to stand up another container running the same process, ether on the same machine or another machine! Not only does it help with failures but it can help with scaling and things like this when you have designed an application to scare horizontally as it can scedule additional containers if needed.

What is horizontal scaling? It’s when you scale by adding more servers, not by migrating to a larger more powerful server, this would be vertical scaling.

## What Kubernetes Is Not

Lets quickly cover what kubenretes is not. It’s not a one stop shop to fix all your deployment issues. It’s not simple, and it brings all it’s own issues. It does not suddenly make your application scale horizontally, Not only this it requires you design your apps in a specific way, to run in a specific way.

Not only does kubernetes add yet another abstraction between your developers and the system it runs in. This means either your developers need to understand kubernetes or you need to build and maintain the infrastructure to abstract this away from the developers.  This is a lot of work.

Kubernetes is a scheduler, one mainly focused on containers. Now while it uses inmutable infrastructure for it's pods/containers this does not mean that it itself manages your infrastructure. The fact is, you are just starting your infrastructure pain when you start using kubernetes. Kubernetes itself is just a group of services that run on a cluster of machines. maintainign and securing those machines as well as ensuring kubernetes itself is up to date and patched along with every base container image you use.

While my goal is not to scare you away, I want to ensure you understand Kubernetes is NOT for everyone. If your application is a monolith, if your application does not scale horizontally or if you have a small team that does not want or need the added complexity, kubernetes is probably not for you.

If you think kubernetes solves your "milti cloud" or "observability" issues. Let me assure you, it does not.

The only problem kubernetes solves, and the one it does well. is the ability to scedule pods/containers. Now there are projects out there like harvestor to make an HCL out of it, istion for service mesh or vector to increase visibility... but you must understand that these are projects built out ON TOP OF. and mostly these are applications that use the control plane to be highly avalible and offer a HA service that does these things. Kubernetes itself does not do this.

And don't be mistaken, the kubernetes ecosystem and community are massive and there are tools to do all sorts of things!

## Technical discription of kubernetes

Lets wrap this lesson up with a solid technical description of kubernetes.

If I was to try and summerize kubernetes into a single sentance to describe what it does I would have to say. Kubernetes is a collection of services to form an application to provide highly avalible and rezilliant sceduling system for groups of related containers. It accomplishes this by leveraging things like etcd, grpc and json. Kubernetes itself, is a collection of micro services that can be run indevidually or on the same machines as the other components.
