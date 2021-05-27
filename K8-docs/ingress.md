# Ingress:

Kubernetes has a built-in configuration object for **HTTP load balancing** called Ingress.

Ingress network is a collection of rules that acts as an entry point to the Kubernetes cluster. This allows inbound connections, 
which can be configured to give services externally through reachable URLs, load balance traffic, or by offering name-based virtual 
hosting. 
So, Ingress is an API object that manages external access to the services in a cluster, usually by HTTP and is the most 
powerful way of exposing service.


## Ingress rules

Each HTTP rule contains an optional host, a list of paths each of which has an associated backend defined with **a serviceName and service port.**

If the traffic path is not matched to any rules, then traffic sends to the default backend.

## Default Backend

The default backend is often a configuration possibility of the Ingress controller and isn’t laid out in your Ingress resources. 
If none of the hosts or methods match the HTTP request within the Ingress objects, the traffic is routed to your default backend.

### Types of Ingress

* Single Service Ingress
* Simple fanout
* Name-based virtual hosting

#### Single Service Ingress

It doesn’t have any rules and it sends traffic to a single service. You can use this to create a default backend with no rules.

```
--- 
apiVersion: extensions/v1beta1
kind: Ingress
metadata: 
  name: frontend-ingress
spec: 
  backend: 
    serviceName: frontend-service
    servicePort: 80
```

#### Simple fanout

A fanout configuration routes traffic to more than one service, based on the HTTP URI being requested.

```
--- 
apiVersion: extensions/v1beta1
kind: Ingress
metadata: 
  name: simple-fanout-example
spec: 
  rules: 
    - 
      host: shopping.example.com
      http: 
        paths: 
          - 
            backend: 
              serviceName: clothes-service
              servicePort: 8080
            path: /clothes
          - 
            backend: 
              serviceName: House-service
              servicePort: 8081
            path: /kitchen

```

#### Name-based virtual hosting

Name-based virtual hosts support routing HTTP traffic to multiple hostnames.

```
--- 
apiVersion: extensions/v1beta1
kind: Ingress
metadata: 
  name: name-virtual-host-ingress
spec: 
  rules: 
    - 
      host: shopping.example.com
      http: 
        paths: 
          - 
            backend: 
              serviceName: clothes-service
              servicePort: 8080
            path: /clothes
          - 
            backend: 
              serviceName: House-service
              servicePort: 8081
            path: /kitchen
    - 
      host: music.example.com
      http: 
        paths: 
          - 
            backend: 
              serviceName: Hindi-service
              servicePort: 9090
            path: /hindhi
          - 
            backend: 
              serviceName: English-service
              servicePort: 9091
            path: /english

```

### Ingress controller

In order to work the ingress resource, the Kubernetes cluster must have an ingress controller running.

It runs as part of the Kube-controller-manager and is typically started automatically with a cluster.

