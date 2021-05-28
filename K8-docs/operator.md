# Operator Pattern

Operators are software extensions to Kubernetes that make use of custom resources to manage applications and their components.
Operators follow Kubernetes principles, notably the **control loop.**

# Operators in Kubernetes 

Kubernetes is designed for automation. Out of the box, you get lots of built-in automation from the core of Kubernetes. You can use Kubernetes to automate deploying and 
running workloads, and you can automate how Kubernetes does that.

Kubernetes' controllers concept lets you extend the cluster's behaviour without modifying the code of Kubernetes itself. Operators are clients of the Kubernetes API that 
act **as controllers for a Custom Resource**.


# Deploying Operators 

The most common way to deploy an Operator is to add the **Custom Resource Definition (CRD)** and its associated Controller to your cluster. The Controller will normally 
run outside of the control plane, much as you would run any containerized application. For example, you can run the controller in your cluster as a Deployment.


# Writing your own Operator 

If there isn't an Operator in the ecosystem that implements the behavior you want, you can code your own.

You also implement an Operator (that is, a Controller) using any language / runtime that can act **as a client for the Kubernetes API.**

Following are a few libraries and tools you can use to write your own cloud native Operator.

* Charmed Operator Framework
* kubebuilder
* KUDO (Kubernetes Universal Declarative Operator)
* Metacontroller along with WebHooks that you implement yourself
* **Operator Framework**[https://github.com/operator-framework/operator-lifecycle-manager/#readme]
*  shell-operator 
