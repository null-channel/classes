# Pod Cheatsheet

Here you can find some of the most useful pod tasks and how to do them via the command line kubectl

##### NOTE:
All of these commands can be run with `-n` or `--namespace` to designate what namesapce the resource you are requesting is in, not all of the commands are demostrated with the namespace as this would become very redundant and is not required if you changed your kubeconfig context to point to the right namespace.

## Lifecycle

```bash
kubectl run [podname] --image=[imagename] # will run the pod on any scedulable node
kubectl run [podname] --image=[imagename] -o yaml --dry-run=client > pod.yaml # will create a yaml file of the pod spec
kubectl delete pod [podname] # delete pod by name in the default namespace
kubectl delete pod [podname] --namespace=[namespace] # delete pod by name, in a namespace
```

## Debugging

```bash
kubectl logs [podname] # get the logs for a single container pod
kubectl logs [podname] -c [container-name] # get the logs from a container in a multi container pod
kubectl describe pod [podname] # describe a pod
```
