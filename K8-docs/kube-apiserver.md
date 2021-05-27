# kube-apiserver

What is the role of kube-apiserver and kube-scheduler?

The kube – apiserver follows the scale-out architecture and, is the front-end of the master node control panel. 
This exposes all the APIs of the Kubernetes Master node components and is responsible for establishing communication
between Kubernetes Node and the Kubernetes master components.

The kube-scheduler is responsible for the distribution and management of workload on the worker nodes. So, it selects 
the most suitable node to run the unscheduled pod based on resource requirement and keeps a track of resource utilization. 
It makes sure that the workload is not scheduled on nodes that are already full.
