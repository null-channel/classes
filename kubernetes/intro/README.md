# A Comprehensive practical introduction to kubernetes 

This class can be found on youtube ---- linky

This class looks to not only be a practical introduction but also provide all the training material you need to start using kubernetes effectivly. While the goal is not about passing or taken exames completing this course should put you well on your way to being able to take an exame such as the CKAD

Below is a quick overview of the lessons to be found here

Lessons marked with +++ have an accompaning katakoda or other lab accompaning them.

## Part 1: Introduction, Basics and exploring
### Lesson 1: What is kubernetes
 - Understanding the need, what containers bring to the table and why you need something like kubernetes to manage them
 - High level overview of what kubernetes is aka. Cluster of machines that orchestrate and manage a group of containers and let you manage the scaling, version and general orchestration of.
 - What kubernetes is not 

### Lesson 2: Understanding Kubernetes nomenclature 
 - What are things like Pod, ReplicaSets, Deployments, namespace, CRD's, operators, Kubectl and CNI

### Lesson 3: How to get a local cluster. +++
 - Using docker on mac
 - Using docker on windows
 - Using minikube and kind on linux
 - How to manually run one with kubeadm - think this is important for learning as it gives you greater freedom for understanding and playing around with things like cluster dns, cni plugin and storage.

### Lesson 4: kubectl - basics +++
 - Understanding the layout of commands
 - Understanding the basic `.kube/config` file
 - Basic understanding of it’s commands “get” and “namespaces”

### Lesson 5: Immutable infrastructure and declarative resources
 - What problems does this solve
 - What issues does it create
 - Declarative resources
 - How does it differ from imperative.

### Lesson 6: Lets run stuff +++
 - Initially a pod, but now we want to scale it,
 - Then a deployment.
 - Then scale it

### Lesson 7: What is a Pod? +++
 - Understanding what a POD is
 - Learn how to tell how many containers are in a pod
 - Learn how to get logs from each container
 - Exec into a container in a multi container POD
 - Light overview of pod networking

### Lesson 8: What is a namespace +++
 - What are kubernetes namespaces
 - Why do they exist
 - How to use them and what uses the provide

### Lesson 9: Jobs and Cronjobs +++
 - How to make them
 - How to use them
 - How to manage them

## Part 2: Networking, Services and Tools

### Lesson 10: Kubernetes Services +++
 - ClusterIP
 - NodePort
 - LoadBalancer
 - Creating a NodePort service locally

### Lesson 11: Kubernetes Services - continued +++
 - What is a LoadBalancer?
 - Creating a LoadBalancer in the cloud or with metalLB
 - Overview of ingress controllers and what issue they solve
 - Intro to headless services

### Lesson 12: Ingress and load balancers +++
 - Ingress tools and what issue they solve
 - how to deploy one and use it to access two different services/app


### Lesson 13: Rolling updates and Rollbacks +++
 - Lets do it

### Lesson 14: Cluster Networking +++
 - Understanding the CNI
 - Discovery of some of the different features of different CNI’s 
 - How to install one using kubeadm

### Lesson 15: Persistent storage - The basics +++
 - How to mount storage into pods
 - What is a PV and PVC at a high level
 - How to attach storage to a pod

### Lesson 16: Persistent Storage - The Storage Class +++
 - What a storage class is
 - How it works
 - Lab on using a storage class to allocate storage in the cloud

### Lesson 17: Logging and Monitoring Nodes, deployments and pods +++
 - Overview of tools - prometheus , netapp
 - Side car concept
 - Visualization tools
 - Log aggregation 
 - Creating a sidecar challenge

### Lesson 18: Config Maps and Secrets +++
 - What they are
 - How to use them
 - Warning about secrets and some technologies (like vault) that can be used to remedy the issues
 - Mounting config maps into your containers

### Lesson 19: Kubectl - Hero mode +++
 - How to use it
 - Understanding how it works
 - Show how to describe stuff you don’t understand
 - Using it to proxy to learn and interact
 - Kubectl config
 - Multi cluster support
 - Using it to debug

### Lesson 20: Scheduling +++
 - Understanding how the scheduler works
 - Using taints and tollorations to adjust how things deploy
 - Understanding that you can use your own custom scheduler if you want (will not cover how to do this in this class, just that it is possible)

## Part 3: Kubernetes Architecture, Debugging

### Lesson 21: OCI, kubelet and how kubernetes runs containers

### Lesson 22: Kubernetes Architecture - How it works - The control plane
 - Api-server
 - Kube scheduler
 - Controller-manager
 - Cloud controller manager

### Lesson 23: Kubernetes Architecture - How it works - the worker nodes +++
 - Kubelet
 - Kube proxy
 - Container runtimes

### Lesson 24: Kubernetes Architecture - How it works - etcd
 - How and why etcd
 - Can be run outside cluster and if really scaling hard, should be

### Lesson 25: Kubernetes Architecture - How it works - Addons +++
 - Networking: flannel/cilium/calico…..
 - Service Discovery: CoreDNS
 - Visualization: Dashboard/WeaveScope
 - Infrastructure: KubeVirt

### Lesson 26: Kubernetes and VM’s +++

### Lesson 27: Debugging containers, pods and services +++
 - Kubectl debug
 - Logs
 - How to figure out why a pod is not running
 - Checking scheduling
 - Checking kubelet

### Lesson 28: Debugging kubernetes clusters +++
 - Checking kubernetes logs
 - Determining if it’s a kubernetes issue
 - Debugging etcd
 - Certs
 - Networking

### Lesson 29: What Kubernetes is not 
 - Understand what issues it can introduce
 - How to deal with those problems
 - How to understand if its what you need.

### Lesson 30: Wrap up 
 - Stress the need to understand how to run stuff on kubernetes, it’s best to work with kubernetes and not against it.
 - Wrap up, at a high level how all the components we talked about interact.
 - Talk about future topics they can investigate, like Storage classes, Headless services, and cloud providers
 - Give a list of projects they can checkout for monitoring and logging and the likes like that.
