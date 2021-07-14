# What is Kubernetes

What is kubernetes you ask? If you are here you might already have an idea, but i’m going to start from the very beginning as I don’t know where each of are starting from, feel free to skip to wherever it becomes interesting for you, though I must warn you, I go in depth on many things and you might learn a thing or two even if you already know a little bit about a subject!

So Lets start from the very start. 

Kubernetes is what a lot of people call a container orchestrator. Now, I would be tempted to argue this is incorrect on a technical level, but it’s an acceptable start. Now why would we want to orchestrate containers?

Well, as we know from our container lessons, containers brought us immutable infrastructure, or part of it at least. It also brought us isolation of workloads. So containers give us the ability to repeatedly spin up the same process on any server we want while also ensuring that our workload does not interfere with another. This is awesome and in the context of microservices where we can have multiple of the same process running... But if we are going to do this, it means a LOT of processes need managed. Not only that but you probably want hardware level redundancy, so for example process x is not all running on machine y, in such a case a failure of machine why would bring all of process x down.

So in the end we can ether higher a fleet of interns to continually check on our applications, fix errors and start running new processes all while updating networking configurations to ensure that where ever these new possesses are created other ones can access them all ensuring our process runs in a distributed way... or we could use something to do this for us! Enter kubernetes. Now while kubernetes solves these issues, and a few others, letting you seamlessly schedule and run containers across a fleet of machines, scaling and auto healing and enabling you to expose those services to other services. It brings with it it's own set of issues that can only be solved by knowledge.

Kubernetes simply clusters a group of servers, and runs containers on those servers. Now, it provides other tools like deployments, replica sets and services, but all these tools are built around helping you manage these containers, or like we learned in the containers class, isolated, repeatable processes. All running on a wide range of servers.

So, kubernetes runs on a cluster of servers, It does this by running a control plane on a small group of servers, 3 to 7, that manages and maintains what is running and where. Now the rest of the machines in the cluster don’t run nearly as much as the control plane and mostly run just a client called the kubelet, and a container runtime, kubeproxy and everything else that will run on these machines will be containers!

This way, if one of these machines goes down, or if one of the container crashes for some reason, kubernetes will work to stand up another container running the same process, ether on the same machine or another machine! Not only does it help with failures but it can help with scaling and things like this when you have designed an application to scare horizontally as it can schedule additional containers if needed.

What is horizontal scaling? It’s when you scale by adding more servers, not by migrating to a larger more powerful server, this would be vertical scaling.

## What Kubernetes Is Not

Lets quickly cover what kubenretes is not. I find it can be helpful to understand what something is not, to better understand what it is. It’s not a one stop shop to fix all your deployment issues. It’s not simple, and it brings all it’s own issues. It does not suddenly make your application scale horizontally, Not only this it requires you design your apps in a specific way, to run in a specific way. It's not your solution to Infrastructure, it does not bring an easy developer experience and lastly it's not a one stop shop, it's the base that things are built on.

Not only does kubernetes add yet another abstraction between your developers and the system it runs in. This means either your developers need to understand kubernetes or you need to build and maintain the infrastructure to abstract this away from the developers.  This is a lot of work. Can slow down development, cause massive errors and downtime if not dwelt with properly. I don't say this to scare you, or tell you I think it's bad. It's not. I say this because too often I see people who miss understand the issues it solves and attempt to fix problems it does not fix while giving themselves larger more painful issues.

Kubernetes is a scheduler, one mainly focused on containers. Now while it uses immutable infrastructure for it's pods/containers this does not mean that it itself manages your infrastructure. The fact is, you are just starting your infrastructure pain when you start using kubernetes. Kubernetes itself is just a group of services that run on a cluster of machines. maintaining and securing those machines, as well as ensuring kubernetes itself is up to date and patched, along with ensuring every base container image you use is patched and up to date is your responsibility. now there are cloud providers that provide services to handle some of these issues and they are awesome, but understand, these are additional features in a managed solution from the cloud providers, not base kubernetes functionality, Kubernetes does not manage your infrastructure, it schedules pods/containers.

While my goal is not to scare you away, I want to ensure you understand Kubernetes is NOT for everyone and not everyone can benefit from adopting it. If your application is a monolith, if your application does not scale horizontally or if you have a small team that does not want or need the added complexity, kubernetes is probably not for you. Microservices are a technique for management of software teams, not a end all be all. If your team does not know kubernetes, and you don't have the resources to train or higher a team to abstract it away from those developers, kubernetes is probably not for you. If your team is small and wants to focus on their app, kubernetes is not for you. If you are struggling to scale and think kubernetes is going to solve this with no changes to your application, kubernets is not for you. There are a lot of other things you can use if any of these are you.

If you think kubernetes solves your "multi-cloud" or "observability" issues. Let me assure you, it does not. Kubernetes can "help" with multi-cloud and it can be a strategy for your multi-cloud, but it is just one peace of the puzzle and requires architecting your app and other things to be "multi cloud". Kubernetes if anything makes observability harder, now there are some tools that help with this, but out of the box, kubernetes, does not do this. It is not it's job.

The only problem kubernetes solves, and the one it does well. is the ability to scedule pods/containers, while it can be used to solve other problems, these are NOT the problems it was designed to solve! Now there are projects out there like harvestor to make an HCL out of it, istio for service mesh or vector to increase visibility... but you must understand that these are projects built out ON TOP OF. and to be honest are mostly applications in their own right that use the control plane to be highly available and offer a HA service that does these things. Kubernetes itself does not do this.

And don't be mistaken, the kubernetes ecosystem and community are massive and there are tools to do all sorts of things! I just want to ensure you understand what kubernetes itself is and does!

## Technical discription of kubernetes

Lets wrap this lesson up with a solid technical description of kubernetes.

If I was to try and summarize kubernetes into a single sentence to describe what it does I would have to say. Kubernetes is a collection of services to form an application to provide highly available and resilient scheduling system for groups of related containers called pods. It accomplishes this by leveraging things like etcd, grpc, a container runtime (think docker or containerd) and json. Kubernetes itself, is a collection of micro services that can be run individually or on the same machines as the other components.

Ok, with this out of the way, lets jump into understanding kubernetes nomenclature and then starting our first cluster to use!
