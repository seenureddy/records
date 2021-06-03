# Configure Liveness, Readiness and Startup Probes

## Liveness Probes

The **kubelet** uses **liveness probes to know when to restart a container**. For example, **liveness probes could catch a deadlock**, where an application is running,
but unable to make progress. Restarting a container in such a state can help to make the application more available despite bugs.

Caution: Liveness probes do not wait for readiness probes to succeed. If you want to wait before executing a liveness probe you should use initialDelaySeconds or a startupProbe.

## Readiness Probes

The **kubelet** uses **readiness probes to know when a container is ready to start accepting traffic**. A Pod is considered ready when all of its containers are ready. 
One use of this signal is to control which Pods are used as backends for Services. When a Pod is not ready, it is removed from Service load balancers.

Probe-level **terminationGracePeriodSeconds cannot** be set for readiness probes. It will be rejected by the API server.
Note: Readiness probes runs on the container during its whole lifecycle.

## Startup Probes

The **kubelet** uses **startup probes to know when a container application has started**. If such a probe is configured, **it disables liveness and readiness checks until 
it succeeds, making sure those probes don't interfere with the application startup.**
**NOTE:** This can be used to adopt liveness checks on slow starting containers, avoiding them getting killed by the kubelet before they are up and running.

## Example Liveness

* To create the pod: `kubectl apply -f exec-liveness.yaml``
* Within 30 seconds, view the Pod events: `kubectl describe pod liveness-exec`
* Wait another 30 seconds, and verify that the container has been restarted: `kubectl get pod liveness-exec`

  ### exec-liveness.yaml
  ```
  apiVersion: v1
  kind: Pod
  metadata:
    labels:
      test: liveness
    name: liveness-exec
  spec:
    containers:
    - name: liveness
      image: k8s.gcr.io/busybox
      args:
      - /bin/sh
      - -c
      - touch /tmp/healthy; sleep 30; rm -rf /tmp/healthy; sleep 600
      livenessProbe:
        exec:
          command:
          - cat
          - /tmp/healthy
        initialDelaySeconds: 5
        periodSeconds: 5
  ```
 The **periodSeconds** field specifies that the kubelet should perform a liveness probe every 3 seconds. The **initialDelaySeconds** field tells the kubelet 
 that it should wait 3 seconds before performing the first probe.
  
 ### liveness HTTP request
 Another kind of liveness probe uses an HTTP GET request. Here is the configuration file for a Pod that runs a container based on the k8s.gcr.io/liveness image.
 
* To create the pod: `kubectl apply -f http-liveness.yaml``
* Within 30 seconds, view the Pod events: `kubectl describe pod liveness-http`
* Wait another 30 seconds, and verify that the container has been restarted: `kubectl get pod liveness-http`

  #### http-liveness.yaml
  ```
  apiVersion: v1
  kind: Pod
  metadata:
    labels:
      test: liveness
    name: liveness-http
  spec:
    containers:
    - name: liveness
      image: k8s.gcr.io/liveness
      args:
      - /server
      livenessProbe:
        httpGet:
          path: /healthz
          port: 8080
          httpHeaders:
          - name: Custom-Header
            value: Awesome
        initialDelaySeconds: 3
        periodSeconds: 3
    ```
### TCP liveness probe 

A third type of liveness probe uses a TCP socket. With this configuration, the kubelet will attempt to open a socket to your container on the specified port. 
If it can establish a connection, the container is considered healthy, if it can't it is considered a failure.


* To create the pod: `kubectl apply -f tcp-liveness-readyness.yaml``
* Within 30 seconds, view the Pod events: `kubectl describe pod goproxy`
* Wait another 30 seconds, and verify that the container has been restarted: `kubectl get pod goproxy`

  #### tcp-liveness-readyness.yml
  ```
  apiVersion: v1
  kind: Pod
  metadata:
    labels:
      app: goproxy
    name: goproxy
  spec:
    containers:
      - name: goproxy
        image: k8s.gcr.io/goproxy:0.1
        ports:
          - containerPort: 8080
        readinessProbe:
          tcpSocket:
            port: 8080
          initialDelaySeconds: 5
          periodSeconds: 10
        livenessProbe:
          tcpSocket:
            port: 8080
          initialDelaySeconds: 15
          periodSeconds: 20
  ```
As you can see, configuration for a TCP check is quite similar to an HTTP check. This example uses both readiness and liveness probes. The kubelet will send the first readiness probe 5 seconds after the container starts. This will attempt to connect to the goproxy container on port 8080. If the probe succeeds, the Pod will be marked as ready. The kubelet will continue to run this check every 10 seconds.

## TCP probes:
 For a TCP probe, the **kubelet makes the probe connection at the node, not in the pod**, which means that you can not use a service name in the 
 host parameter since the kubelet is unable to resolve it.

### Configure Probes
Probes have a number of fields that you can use to more precisely control the behavior of liveness and readiness checks:

**initialDelaySeconds**: Number of seconds after the container has started before liveness or readiness probes are initiated. Defaults to 0 seconds. Minimum value is 0.

**periodSeconds**: How often (in seconds) to perform the probe. Default to 10 seconds. Minimum value is 1.

**timeoutSeconds**: Number of seconds after which the probe times out. Defaults to 1 second. Minimum value is 1.

**successThreshold**: Minimum consecutive successes for the probe to be considered successful after having failed. Defaults to 1. Must be 1 for liveness and 
                      startup Probes. Minimum value is 1.
                      
**failureThreshold**: When a probe fails, Kubernetes will try failureThreshold times before giving up. Giving up in case of liveness probe means restarting the 
                      container. In case of readiness probe the Pod will be marked Unready. Defaults to 3. Minimum value is 1.
                   

