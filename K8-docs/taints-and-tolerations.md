# Taints and Tolerations:

https://medium.com/kubernetes-tutorials/making-sense-of-taints-and-tolerations-in-kubernetes-446e75010f4e

taints and tolerations, node affinity and pod affinity, anti-affinity work and can be used to instruct the 
Kubernetes scheduler to place pods on nodes that fulfill their special needs.


This Kubernetes feature allows users to mark a node (taint the node) so that no pods can be scheduled to it, 
unless a pod explicitly tolerates the taint. Using this Kubernetes feature we can create nodes that are reserved 
(dedicated) for specific pods. E.g. pods which require that most of the resources of the node be available to them 
in order to operate flawlessly should be scheduled to nodes that are reserved for them.

Working with Taints and Tolerations

The process of adding taints and tolerations to nodes and Pods is similar to how node affinity works.

First, we add a taint to a node that should repel certain Pods. For example, if your node’s name is host1 , 
you can add a taint using the following command:

```EX:
kubectl taint nodes host1 special=true:NoSchedule
node “host1” tainted
```
The taint has the format <taintKey>=<taintValue>:<taintEffect> . Thus, the taint we just created has the key 
“special“, the value “true“, and the taint effect NoSchedule. A taint’s key and value can be any arbitrary string 
and the taint effect should be one of the supported taint effects such as NoSchedule , NoExecute , and PreferNoSchedule .

Taint effects define how nodes with a taint react to Pods that don’t tolerate it. For example, the NoSchedule taint
 effect means that unless a Pod has matching toleration, it won’t be able to schedule onto the host1 . Other supported
 effects include PreferNoSchedule and NoExecute . The former is the “soft” version of NoSchedule . If the PreferNoSchedule 
 is applied, the system will try not to place a Pod that does not tolerate the taint on the node, but it is not required. 
 Finally, if the NoExecute effect is applied, the node controller will immediately evict all Pods without the matching 
 toleration from the node.

You can verify that the taint was applied by running kubectl describe nodes <your-node-name> and checking the Taints 
section of the response:

```EX:
kubectl describe nodes host1
```

If you don’t need a taint anymore, you can remove it like this:

```EX:
kubectl taint nodes host1 special:NoSchedule-
node “host1” untainted
```

## Adding Tolerations:

As you already know, taints and tolerations work together. Without a toleration, no Pod can be scheduled onto a node with a taint. 
That’s not what we trying to achieve! Let’s now create a Pod with a toleration for the taint we created above. Tolerations are specified 
in the PodSpec:

```
EX:
apiVersion: v1
kind: Pod
metadata:
  name: pod-1
  labels:
    security: s1
spec:
  containers:
  - name: bear
    image: supergiantkir/animals:bear
  tolerations:
  - key: "special"
    operator: "Equal"
    value: "true"
    effect: "NoSchedule"
```
As you see, the Pod’s toleration has the key “special“, the value “true“, and the effect “NoSchedule“, which exactly matches 
the taint we applied earlier. Therefore, this Pod can be scheduled onto the host1 . However, this does not mean that the Pod 
will be scheduled onto that exact node because we did not use node affinity or nodeSelector .

The second Pod below can be also scheduled onto a tainted node although it uses the operator “Exists” and does not have the key’s value defined.
```
EX:
apiVersion: v1
kind: Pod
metadata:
  name: pod-2
  labels:
    security: s1
spec:
  containers:
  - name: bear
    image: supergiantkir/animals:bear
tolerations:
- key: "special"
  operator: "Exists"
  effect: "NoSchedule"
  ```

This demonstrates the following rule: if the operator is Exists the toleration matches the taint if keys and effects are the same(no value should be specified). 
However, if the operator is Equal , the toleration’s and taint’s values should be also equal.


NoExecute Effect

The taint with the NoExecute effect results in the eviction of all Pods without a matching toleration from the node. 
When using the toleration for the NoExecute effect you can also specify an optional tolerationSeconds field. Its value defines 
how long the Pod that tolerates the taint can run on that node before eviction and after the taint is added. Let’s look at the manifest below:
```
EX:

apiVersion: v1
kind: Pod
metadata:
  name: pod-2
  labels:
    security: s1
spec:
  containers:
  - name: bear
    image: supergiantkir/animals:bear
  tolerations:
  - key: "key1"
    operator: "Equal"
    value: "value1"
    effect: "NoExecute"
    tolerationSeconds: 3600
```
If this Pod is running and a matching taint is added to the node, it will stay bound to the node for 3600 seconds. If the taint is removed by
that time, the Pod won’t be evicted.


In general, the following rules apply for the NoExecute effect:

    Pods with no tolerations for the taint(s) are evicted immediately.
    Pods with the toleration for the taint but that do not specify tolerationSeconds in their toleration stay bound to the node forever.
    Pods that tolerate the taint with a specified tolerationSeconds remain bound for the specified amount of time.
