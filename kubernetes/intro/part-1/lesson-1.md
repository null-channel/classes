# What is Kubernetes

What is kubernetes? If you are here you might already have an idea, but i’m going to start from the very beginning as I don’t know where you each are starting from, feel free to skip to wherever it becomes interesting for you!

So Lets start from the very start. 

Kubernetes is what a lot of people call a container orchestrator. Now, I would be tempted to argue this is incorrect on a technical level, but it’s an acceptable start. Now why would we want to orchestrate containers?

Well, as we know from our container lessons, containers brought us immutable infrastructure, or part of it at least. So containers give us the ability to repeatedly spin up the same process on any server we want. But now the issue we face is managing a bunch of processes running on a bunch of servers, and if one of those servers where to fail you need to reschedule that process, or container on another server. This is the core problem kubernetes looks to solve.

Kubernetes clusters a group of servers, and runs containers on them. Now, it provides other tools like deployments, replica sets and services, but all these tools are built around helping you manage these containers running on a wide range of servers.

Lets quickly start with what kubenretes is not. It’s not a one stop shop to fix all your deployment issues. It’s not simple, and it brings all it’s own issues. Not only this it requires you design your apps in a specific way, to run in a specific way.

Not only this kubernetes adds yet another abstraction between your developers and the system it runs in. This means either your developers need to understand kubernetes or you need to build and maintain the infrastructure to abstract this away from the developers.  This is a lot of work and does not make any since unless you not only have large teams but also be ready to design your applications in a cloud native way.

So, kubernetes runs on a cluster of servers, It does this by running a control plane on a small group of servers, 3 to 7, that manages and maintains what is running and where. Now the rest of the machines in the cluster don’t run nearly as much as the control plane and mostly run just a client called the kubelet, and a container runtime, everything else that will run on these machines will be containers!

This way, if one of these machines goes down, or if one of the container crashes for some reason, kubernetes will work to stand up another container running the same process, ether on the same machine or another machine! Not only does it help with failures but it can help with scaling and things like this when you have designed an application to scare horizontally.

What is horizontal scaling? It’s when you scale by adding more servers, not by migrating to a larger more powerful server, this would be vertical scaling.

## What Kubernetes Is Not



## Technical discription of kubernetes
