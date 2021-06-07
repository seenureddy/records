# SDN
https://www.gavstech.com/software-defined-networking-sdn/

As per Open Networking Foundation’s definition, Software-Defined Networking is the physical separation of the network control plane from the forwarding plane 
and where a control plane controls several devices.

In a traditional network architecture, individual network devices make traffic decisions (control plane) and forward packets/frames from one interface to 
another (data plane). Thus, they have all functions and processes related to both control plane and data plane.

But in Software-Defined Networking, the control plane and data plane are decoupled. The control plane is implemented in software which helps the network 
administrator to manage the traffic programmatically from a centralized location. The added advantage is that individual switches in the network do not 
require intervention of the network administrator to deliver the network services.

Software-Defined networking (SDN), makes networks agile and flexible. It provides better network control and hence enables the cloud computing service
providers to respond quickly to ever changing business requirements. In SDN, the underlying infrastructure is abstracted for applications and network
services.

## SDN architecture

A typical representation of SDN architecture includes **three layers: the application layer, the control layer and the infrastructure layer.**

The SDN application layer, not surprisingly, contains the typical network applications or functions like intrusion detection systems, load balancing
or firewalls. A traditional network uses a specialized appliance, such as a firewall or load balancer, whereas a software-defined network replaces
the appliance with an application that uses the controller to manage the data plane behaviour.

SDN architecture separates the network into three distinguishable layers, i.e., applications communicate with the control layer using northbound 
API and control layer communicates with data plane using southbound APIs. The control layer is considered as the brain of SDN. The intelligence 
to this layer is provided by centralized SDN controller software. This controller resides on a server and manages policies and the flow of traffic
throughout the network. The physical switches in the network constitute the infrastructure layer. 

