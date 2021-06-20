# Understanding Basic Kubernets Nomenclature

So before we dive headfirst into kubernetes I think it would be best to quickly cover some of the names and concepts at a high level, that way when I say something like “a pod” you don’t imidiatly think I mean some place to take a nap. Be warned, I don't expect all of these things to make since just now. I just thought it would be helpful for you to have some idea about what they mean.

Kubernetes has a few core concepts, tools and services, These are:

Pods, ReplicaSets, Deployments, Namespaces, Services, Kubectl, CRD’s, Kubernetes control plane, nodes and kubelet

Now before we talk about these, let me clear something up, a lot of these are kubernetes resources, a kubernetes resource is just a simple resource that can be managed by the command line using declaritive files. More on declarative vs imparative later on, but lets understand right now. You don't create containers with kubernetes, but instead you describe, usually using a file, how many and of what type of container you want running, and it works to do it. These are those resources, those files.

Pods are what kubernetes actually orchestrates, Most people think kubernetes is a container orchestrator, but it is not. It’s a pod orchestrator. This being said, a pod is a collection of one or more tightly coupled containers. These containers can, and do communicate with each other like they were running on the same host (aka, localhost). We will delve further into running, and managing pods but this is their base concept.

ReplicaSets and Deployments are used mostly together and work to achieve a singular goal of managing deployments of pods. We will get into these more later and why there are both deployments and replicasets but suffice to say that these, especially deployments are what are used by kubernetes to manage deployments of pods.

Namespaces are a tool used to seperate and isolate resources in a kubernetes cluster. These can be used for a lot of things but for right now all you really need to know is there are two main ones when you have a new cluster, kube-system, where all the kubernetes system components run and `default` the default namespace where things go if you don't specify one. In general, the default namespace should only ever be used for playing around never in production.

Services are the type of resource used to expose your deployments and pods in kubernetes. This maps trafic to wherever your pods are running. If you think about it, the entire goal of kubernetes is to make it so you don't have to care about where exactly, your pod is running. It can handle machine/node faults, but to do this it might scedule your pod on a new machine. This being said you don't know where your pod is running and the service handles mapping your request to your pods.

Nodes are your machines. these could be bare metal, these could be vms, these can even be containers themselves though that really is not a production way to run. A kubernetes node is just an instance where the kubelet is running and kubernetes can scedule containers to run.

Kubectl, it comes under many names, kubeCTL, kubecuttle or the kubernetes cli. this is the primary command line tool used to interact with the kubernetes control plane.

CRDs are custom resource definitions, these let you implement your own kubernetes object like the ones mentioned above. This is a much more advanced topic and will not be covered more in depth till much later on. But I wanted to make sure you knew what this term was in case you heard it thrown around.

Kubernetes control plane, this is where the api server lives as well as the gut's of kubernetes. The control plane is where the real magic of kubernetes happens, but usually does not run any of the containers on it and delegates this task out to the worker nodes.

Kubelet is the service that runs on each of the worker nodes and manages the pods and containers running on it. It's the service that interacts with the container runtime and reports back to the control plane on their status

Ok, so those are some of the basic, and no so basic, resources you can interact with in kubernetes. there are many more but these are the ones you will learn about more indepth in this class, understand that all of these resources can and do have their state described in yaml or json files and all of these resources are used to deploy, manage, scale, secure and serve your application!

to sumerize all of this; deployments and replicasets manage pods that manage contianers on a node, services expose these pods, namespaces seperate your pods, services and deployments while crd's are just ways people can implement their own resources and explain them to kubernetes. kubectl is the command line tool we will be hevilly using to interact with the kubernetes api that runs in the kubernetes control plane to create, update and delete any one of these resources.

So I know that some of that probably did not make since. And that is ok. my hope is that in the future you will be predisposed to some of these iteams and the puzzle peices I laid out will start to fit together.