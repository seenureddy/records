# POD
### Container dependencies and startup order in Pod

All containers in a Pod are being started in parallel and there is no way to define that one container must be started after other container. 
For example, in the IPC example, there is a chance that the second container might finish starting before the first one has started and created 
the message queue. In this case, the second container will fail, because it expects that the message queue already exists.

In a cloud native environment, it’s always better to plan for failures outside of your immediate control.  For example, one way to to fix this issue
would be to change the application to wait for the message queue to be created.

`The recommended way to install application dependencies in Kubernetes is through **Init Containers**. Init Containers are defined in the pod deployment,
and they block the start of the application until they run successfully.`

```

apiVersion: v1
kind: Pod
metadata:
  name: myapp-pod
  labels:
    app: myapp
spec:
  containers:
  - name: myapp-container
    image: busybox:1.28
  initContainers:
  - name: init-myservice
    image: busybox:1.28
  - name: init-mydb
    image: busybox:1.28
 
```
