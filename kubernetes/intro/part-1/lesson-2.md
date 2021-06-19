# Understanding Basic Kubernets Nomenclature

So before we dive headfirst into kubernetes I think it would be best to quickly cover some of the names and concepts at a high level, that way when I say something like “a pod” you don’t think I mean some place to take a nap.

Kubernetes has a few core concepts, These are:

Pods, ReplicaSets, Deployments, Namespaces, Services, Kubectl, CRD’s 

Now before we talk about these, let me clear up, a lot of these are kubernetes resources, a kubernetes resource is just a simple resource that can be managed my the command line using declaritive files. More on declarative vs imparative later on.

Pods are what kubernetes actually orchestrates, Most people think kubernetes is a container orchestrator, but it is not. It’s a pod orchestrator. This being said a pod is a collection of one or more tightly coupled containers. These containers can, and do communicate with each other like they were running on the same host (aka, localhost). We will delve further into running, and managing pods but this is their base concept.

ReplicaSets and Deployments are used in the most part together and are used to achieve a singular goal of managing deployments of pods. We will get into these more later but suffice to say that these, especially deployments are what are used by kubernetes to manage deployments of pods.

Namespaces are a tool used to seperate and isolate resources in a kubernetes cluster. These can be used for a lot of things but for right now all you really need to know is there are two main ones when you have a new cluster, kube-system, where all the kubernetes system components run and `default` the default namespace where things go if you don't specify one. In general, the default namespace should only ever be used for playing around never in production.

Services are the type of resource used to expose your deployments and pods in kubernetes. This maps trafic to wherever your pods are running. If you think about it, the entire goal of kubernetes is to make it so you don't have to care about where exactly, your pod is running. It can handle faults, but to do this it might scedule your pod on a new machine. This being said you don't know where your pod is running and the service handles mapping your request to your pods.

Kubectl, it comes under many names, kubeCTL, kubecuttle or the kubernetes cli. this is the primary command line tool used to interact with the kubernetes control plane.

CRDs are custom resource definitions, these let you implement your own kubernetes object like the ones mentioned above. This is a much more advanced topic and will not be covered more in depth till much later on. But I wanted to make sure you knew what this term was in case you heard it thrown around.

Ok, so those are some of the basic resources you can interact with in kubernetes. there are many many more but these are the basic ones you will learn about in this class, understand that all of these resources can and do have their state described in yaml or json files and all of these resources are used to deploy, manage, scale, secure and serve your application!