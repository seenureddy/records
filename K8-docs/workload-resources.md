# Workload Resources

Deployments, ReplicaSet, StatefulSets, DaemonSet, Jobs, Garbage Collection, TTL Controller for Finished Resources, CronJob, ReplicationController

## DaemonSet

A DaemonSet ensures that all (or some) Nodes run a copy of a Pod. As nodes are added to the cluster, Pods are added to them. As nodes are removed 
from the cluster, those Pods are garbage collected. Deleting a DaemonSet will clean up the Pods it created.

Some typical uses of a DaemonSet are:

   * running a cluster storage daemon on every node
   * running a logs collection daemon on every node
   * running a node monitoring daemon on every node

In a simple case, one DaemonSet, covering all nodes, would be used for each type of daemon. A more complex setup might use multiple DaemonSets for 
a single type of daemon, but with different flags and/or different memory and cpu requests for different hardware types.

## StatefulSets

StatefulSet is the workload API object used to manage stateful applications.
Manages the deployment and scaling of a set of Pods, and provides guarantees about the ordering and uniqueness of these Pods.

Like a Deployment, a StatefulSet manages Pods that are based on an identical container spec. Unlike a Deployment, a StatefulSet maintains a sticky 
identity for each of their Pods. These pods are created from the same spec, but are not interchangeable: each has a persistent identifier that it 
maintains across any rescheduling.

If you want to use storage volumes to provide persistence for your workload, you can use a StatefulSet as part of the solution. Although individual
Pods in a StatefulSet are susceptible to failure, the persistent Pod identifiers make it easier to match existing volumes to the new Pods that replace 
any that have failed.

 ### Using StatefulSets 
 StatefulSets are valuable for applications that require one or more of the following.
    Stable, unique network identifiers.
    Stable, persistent storage.
    Ordered, graceful deployment and scaling.
    Ordered, automated rolling updates.

 In the above, stable is synonymous with persistence across Pod (re)scheduling. If an application doesn't require any stable identifiers or ordered 
 deployment, deletion, or scaling, you should deploy your application using a workload object that provides a set of stateless replicas. **Deployment 
 or ReplicaSet may be better suited to your stateless needs.**


## ReplicaSet

A ReplicaSet's purpose is to maintain a stable set of replica Pods running at any given time. As such, it is often used to guarantee the availability 
of a specified number of identical Pods.


## Garbage Collection

The role of the Kubernetes garbage collector is to delete certain objects that once had an owner, but no longer have an owner.

 ### Owners and dependents 
 Some Kubernetes objects are owners of other objects. For example, a ReplicaSet is the owner of a set of Pods. The owned objects are called dependents 
 of the owner object. Every dependent object has a metadata.ownerReferences field that points to the owning object.


