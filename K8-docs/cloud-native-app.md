
Cloud native is an approach for building applications as micro-services and running them on a containerised and dynamically orchestrated 
platforms that fully exploits the advantages of the **cloud computing model**. Cloud-native is about how applications are created and deployed, 
not where. 

Cloud-native development focuses on an architecture’s modularity, loose coupling, and the independence of its services. Each microservice 
implements a business capability, runs in its own process, and communicates via application programming interfaces (APIs) or messaging. 
This communication can be managed through **a service mesh layer**.

# Cloud Native Design Principles

* **Packaged As Lightweight Containers And Orchestrated**
* **Agile DevOps & Automation Using CI/CD**
* **Elastic — Dynamic scale-up/down**
* **Designed As Loosely Coupled Microservices**
Microservice is an approach to develop a single application as a suite of small services, each running in their own process and communicating using 
lightweight protocols like HTTP. 

* **Developed With Best-of-breed Languages And Frameworks**
Each service of a cloud-native application is developed using the language and framework best suited for the functionality. **Cloud-native applications 
are polyglot.** Services use a variety of languages, runtimes and frameworks. 
For example, developers may build a real-time streaming service based on WebSockets, developed in Node.js, while choosing Python for building 
a machine learning based service and choosing spring-boot for exposing the REST APIs. The fine-grained approach to developing microservices 
lets them choose the best language and framework for a specific job.

* **Centred Around APIs For Interaction And Collaboration**

Cloud-native services use lightweight APIs that are based on protocols such as representational state transfer (REST) to expose their functionality. 
Internal services communicate with each other using **binary protocols like Thrift, Protobuff, GRPC etc** for better performance.

* **Stateless And Massively Scalable**

A cloud-native app stores its state in **a database or some other external entity so instances can come and go**. Any instance can process a request.
They are not tied to the underlying infrastructure which allows the app to run in a highly distributed manner and still maintain its state 
independent of the elastic nature of the underlying infrastructure. From a scalability perspective, the architecture is as simple as just by adding 
commodity server nodes to the cluster, it should be possible to scale the application

* **Resiliency At The Core Of the Architecture**

According to Murphy’s law — “Anything that can fail will fail”. When we apply this to software systems, In a distributed system, failures will happen. 
Hardware can fail. The network can have transient failures. Rarely, an entire service or region may experience a disruption, but even those must be 
planned for. Resiliency is the ability of a system to recover from failures and continue to function. It’s not about avoiding failures, but responding 
to failures in a way that avoids downtime or data loss. The goal of resiliency is to return the application to a fully functioning state following a 
failure. Resiliency offers the following:

    **High Availability** — the ability of the application to continue running in a healthy state, without significant downtime
    **Disaster Recovery** — the ability of the application to recover from rare but major incidents: non-transient, wide-scale failures, such as service 
    disruption that affects an entire region
   
One of the main ways to make an application resilient is through redundancy. HA and DR are implemented using multi node clusters, Multi region deployments,
data replication, no single point of failure, continuous monitoring etc.


Following are some of the strategies for implementing resiliency:

 **Retry transient failures** — Transient failures can be caused by momentary loss of network connectivity, a dropped database connection, or a timeout
   when a service is busy. Often, a transient failure can be resolved simply by retrying the request.

**Load balance across instances** — Implement cluster everywhere. Stateless applications should be able to scale by adding more nodes to the cluster

**Degrade gracefully** — If a service fails and there is no failover path, the application may be able to degrade gracefully while still providing 
    an acceptable user experience

**Throttle high-volume tenants/users** — Sometimes a small number of users create excessive load. That can have an impact on other users, reducing 
   the overall availability of your application

**Use a circuit breaker** — The circuit breaker pattern can prevent an application from repeatedly trying an operation that is likely to fail. 
   The circuit breaker wraps calls to a service and tracks the number of recent failures. If the failure count exceeds a threshold, the circuit
   breaker starts returning an error code without calling the service
 
**Apply compensating transactions**. A compensating transaction is a transaction that undoes the effects of another completed transaction. 
In a distributed system, it can be very difficult to achieve strong transactional consistency. Compensating transactions are a way to achieve 
consistency by using a series of smaller, individual transactions that can be undone at each step.
  
**Testing for resiliency** — Normally resiliency testing cannot be done the same way that you test application functionality (by running unit tests, 
integration tests and so on). Instead, you must test how the end-to-end workload performs under failure conditions which only occur intermittently. 
For example: inject failures by crashing processes, expired certificates, make dependent services unavailable etc. **Frameworks like chaos monkey can 
be used for such chaos testing.**





