# Deployment Cheatsheet

Here you can find some of the most useful deployment tasks and how to do them via the command line kubectl

## lifecycle

```bash
kubectl create deployment [name] --image=[name] # create a deployment
kubectl create deployment [name] --image=[name] --replicas=3 --dry-run=client -o yaml > deployment.yaml # create a deployment yaml file with 3 replicas
```
