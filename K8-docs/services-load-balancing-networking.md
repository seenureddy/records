# Services, Load Balancing, and Networking

Kubernetes networking addresses four concerns:

    Containers within a Pod use networking to communicate via loopback.
    Cluster networking provides communication between different Pods.
    The Service resource lets you expose an application running in Pods to be reachable from outside your cluster.
    You can also use Services to publish services only for consumption inside your cluster.

## Service

**An abstract way to expose an application running on a set of Pods as a network service.**
With Kubernetes you don't need to modify your application to use an unfamiliar service discovery mechanism. Kubernetes gives 
**Pods their own IP addresses and a single DNS name for a set of Pods,** and can load-balance across them.

