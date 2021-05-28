# Custom Resources

Custom resources are extensions of the Kubernetes API. Discusses here when to add a custom resource to your Kubernetes cluster and when 
to use a standalone service. It describes the two methods for adding custom resources and how to choose between them.

A resource is an endpoint in the Kubernetes API that stores a collection of API objects of a certain kind; for example, the built-in pods 
resource contains a collection of Pod objects.

A custom resource is an extension of the Kubernetes API that is not necessarily available in a default Kubernetes installation. It represents
a customization of a particular Kubernetes installation. However, many core Kubernetes functions are now built using custom resources, making 
Kubernetes more modular.

Custom resources can appear and disappear in a running **cluster through dynamic registration**, and cluster admins can update custom resources 
independently of the cluster itself. Once a custom resource is installed, users can create and access its objects using kubectl, just as they 
do for built-in resources like Pods.

# Custom controllers

On their own, custom resources let you store and retrieve structured data. When you combine a custom resource with a custom controller, 
**custom resources provide a true declarative API.**

**A declarative API** allows you to declare or specify the **desired state of your resource and tries to keep the current state of Kubernetes 
objects in sync with the desired state.** The controller interprets the structured data as a record of the user's desired state, and 
continually maintains this state.

You can deploy and update a custom controller on a running cluster, independently of the cluster's lifecycle. Custom controllers can work with 
any kind of resource, but they are especially effective when combined with custom resources. The **Operator pattern** combines custom resources
and custom controllers. You can use custom controllers to encode domain knowledge for specific applications into an extension of the Kubernetes API.


## Adding custom resources 

two ways to add custom resources to your cluster:
* CRDs are simple and can be created without any programming.
* **API Aggregation (AA)** requires programming, but allows more control over API behaviors like how data is stored and conversion between API versions.

Aggregated APIs are subordinate API servers that sit behind the primary API server, which acts as a proxy. This arrangement is called API Aggregation (AA). 
To users, the Kubernetes API appears extended.

CRDs allow users to create new types of **resources without adding another API server.** You do not need to understand API Aggregation to use CRDs.

Regardless of how they are installed, the new resources are referred to as Custom Resources to distinguish them from built-in Kubernetes resources (like pods).


# CustomResourceDefinitions (CRD)

The **CustomResourceDefinition API** resource allows you to define custom resources. Defining a CRD object creates a new custom resource with a name and schema
that you specify. The Kubernetes API serves and handles the storage of your custom resource. The name of a CRD object must be a valid **DNS subdomain name.**

This frees you from writing your own API server to handle the custom resource, but the generic nature of the implementation means you have less flexibility 
than with **API server aggregation.**

Refer to the **custom controller example(https://github.com/kubernetes/sample-controller)** for an example of how to register a new custom resource, work with instances of your new resource type, and use a controller 
to handle events. 

# API server aggregation

Usually, each resource in the Kubernetes API requires code that handles REST requests and manages persistent storage of objects. The main Kubernetes API server
handles built-in resources like pods and services, and can also generically handle custom resources through **CRDs.**

The **aggregation layer** allows you to provide specialized implementations for your custom resources by writing and deploying your own standalone API server. The 
main API server delegates requests to you for the custom resources that you handle, making them available to all of its clients.






