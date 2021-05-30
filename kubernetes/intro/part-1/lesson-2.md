# Understanding Basic Kubernets Nomenclature

So before we dive headfirst into kubernetes I think it would be best to quickly cover some of the names and concepts at a high level, that way when I say something like “a pod” you don’t think I mean some place to take a nap.

Kubernetes has a few core concepts, These are:

Pods, ReplicaSets, Deployments, Namespaces, Services, Kubectl, CRD’s 

Pods are what kubernetes actually orchestrates, Most people think kubernetes is a container orchestrator, but it is not. It’s a pod orchestrator. This being said a pod is a collection of one or more tightly coupled containers. These containers can, and do communicate with each other like they were running on the same host (aka, localhost). We will delve further into running, and managing pods but this is their base concept.

ReplicaSets and Deployments are used in the most part together and are used to achieve a singular goal of managing deployments of pods. We will get into these more later but suffice to say that these, especially deployments are what are used by kubernetes to manage 
