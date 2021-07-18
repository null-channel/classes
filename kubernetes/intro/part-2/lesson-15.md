# Lesson 15: NetworkPolicy

Network Policies is a resource you can create in kubernetes, just like Deployments and services. These themselves don't acctually do anything, that being said you network provider (calico or cilium for example) bring the functionality to use and implement them!

The entities that a Pod can communicate with are identified through a combination of the following 3 identifiers:

Other pods that are allowed (exception: a pod cannot block access to itself)
Namespaces that are allowed
IP blocks (exception: traffic to and from the node where a Pod is running is always allowed, regardless of the IP address of the Pod or the node)

By default, pods are non-isolated; they accept traffic from any source.

Network policies do not conflict; they are additive. If any policy or policies select a pod, the pod is restricted to what is allowed by the union of those policies' ingress/egress rules. Thus, order of evaluation does not affect the policy result.
// TODO: Explain this

```yaml
apiVersion: networking.k8s.io/v1
kind: NetworkPolicy
metadata:
  name: test-network-policy
  namespace: default
spec:
  podSelector:
    matchLabels:
      role: db
  policyTypes:
  - Ingress
  - Egress
  ingress:
  - from:
    - ipBlock:
        cidr: 172.17.0.0/16
        except:
        - 172.17.1.0/24
    - namespaceSelector:
        matchLabels:
          project: myproject
    - podSelector:
        matchLabels:
          role: frontend
    ports:
    - protocol: TCP
      port: 6379
  egress:
  - to:
    - ipBlock:
        cidr: 10.0.0.0/24
    ports:
    - protocol: TCP
      port: 5978
```
// TODO: Explain this
deny all example:
```yaml
apiVersion: networking.k8s.io/v1
kind: NetworkPolicy
metadata:
  name: default-deny-ingress
spec:
  podSelector: {}
  policyTypes:
  - Ingress
```
// TODO: Explain this
Another example with egress cidr and ports

```yaml
apiVersion: networking.k8s.io/v1
kind: NetworkPolicy
metadata:
  name: multi-port-egress
  namespace: default
spec:
  podSelector:
    matchLabels:
      role: db
  policyTypes:
  - Egress
  egress:
  - to:
    - ipBlock:
        cidr: 10.0.0.0/24
    ports:
    - protocol: TCP
      port: 32000
      endPort: 32768
```


// TODO: Things it can't do just yet:
Forcing internal cluster traffic to go through a common gateway (this might be best served with a service mesh or other proxy).
Anything TLS related (use a service mesh or ingress controller for this).
Node specific policies (you can use CIDR notation for these, but you cannot target nodes by their Kubernetes identities specifically).
Targeting of services by name (you can, however, target pods or namespaces by their labels, which is often a viable workaround).
Creation or management of "Policy requests" that are fulfilled by a third party.
Default policies which are applied to all namespaces or pods (there are some third party Kubernetes distributions and projects which can do this).
Advanced policy querying and reachability tooling.
The ability to log network security events (for example connections that are blocked or accepted).
The ability to explicitly deny policies (currently the model for NetworkPolicies are deny by default, with only the ability to add allow rules).
The ability to prevent loopback or incoming host traffic (Pods cannot currently block localhost access, nor do they have the ability to block access from their resident node).


That being said you can do those things in things like your service mesh, or even some CNI solutions like Cilium.

// TODO: Why and when you should use them