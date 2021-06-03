# Configure Liveness, Readiness and Startup Probes

## Liveness Probes

The **kubelet** uses **liveness probes to know when to restart a container**. For example, **liveness probes could catch a deadlock**, where an application is running,
but unable to make progress. Restarting a container in such a state can help to make the application more available despite bugs.


## Readiness Probes

The **kubelet** uses **readiness probes to know when a container is ready to start accepting traffic**. A Pod is considered ready when all of its containers are ready. 
One use of this signal is to control which Pods are used as backends for Services. When a Pod is not ready, it is removed from Service load balancers.


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

