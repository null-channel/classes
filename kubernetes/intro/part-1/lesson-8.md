# What is a namespace

Ok. I bring up namespaces because lots of times when you deploy something from a helm chart or the likes it's going to deploy into it's own namespace and it's important to understand how to use them even if you don't actively use them. Namespaces primary use are for environments with larger teams and provide a sort of "soft, trusted multitenancy" they are not the solution to trusted multitenancy but they help with it. So think of a large company with lost of teams that use a single cluster. If you are running a smaller team there is no need to use namespaces or care about them.

Lets sum up what namespaces are at a high level, They are the exact same as namespaces in languages like c#, the linux kernel and others. Namespaces provide a scope for names, meaning that names, like your deployment name `nginx` in the last example needs to be unique WITHIN its namespace. And that happened to be the "default" namespace.

Ok. lets do a quick overview of the default namespaces that are created with most (all?) Kubernetes clusters.

list them by using the same way we get all cabernets resources `kubectl get namespaces`

Note here: if you are running on kind or minikube there will/can be a few other namespaces for storage related things. This is fine! don't worry! your cluster could have more depending on where you deployed it!

```
NAME              STATUS   AGE
default           Active   1d
kube-node-lease   Active   1d
kube-public       Active   1d
kube-system       Active   1d
```

`default` is well... the default namespace! it's the one that our deployment went in because we did not specify a namespace!
`kube-node-leases` - you don't really need to worry about this right now... to sum it up it helps manage node leases to improve heartbeat functionality when your cluster scales and starts to become a larger cluster!
`kube-public` - is used for to enable kubernetes to make some resources visible and readable publicly throughout the entire cluster
`kube-system` is the namespace where most of the "kubernetes" bits run. when you run a `kubeadm` cluster this is where it is going to run all the little bits of the kubernetes system like the api server.

ok. so every `kubectl` command can be passed a `--namespace=[name]` or `-n [name]` this will run that command in that namespace. lets test this out.

first run `kubectl get pods`
now run `kubectl get pods -n kubesystem`
now you can see all the pods running in the kubesystem!

Now we want to create a namespace? `kubectl create namesapce nginx` lets go ahead and list the namespaces `kubectl get namespaces`
cool. we created our namespace.

Lets take a look at that deployment file we created in the last lesson

```
apiVersion: apps/v1
kind: Deployment
metadata:
  creationTimestamp: null
  labels:
    app: nginx
  name: nginx
```

We can add a new felid here `namespace: nginx` and then apply this file `kubectl apply -f nginx-deployment.yaml -n nginx` and there you go. an nginx deployment in your new namespace nginx.

You can check on it's pod by running `kubectl get pods -n nginx`!

I have one more secret to show you. you can pass the `-A` to kubectl to run on all namespaces!

This is pretty much all you need to know about namesapces for now! have fun!