# Init Containers

Created: March 2, 2023 3:06 PM

# What is Init Container?

A Pod can have multiple containers within the same pod, including init containers. Init Containers are a type of container that runs before the Pod’s main containers start.

**Important:** If Init containers fail, the Pod’s main containers will fail.

# Why use Init Containers?

The main use case of Init Containers is to perform any initialization tasks that are required before the main application containers can start running.

For example, an Init Container can be used to:

1. Fetch configuration files or data required by the main application container.
2. Initialize a database or other required services.
3. Perform any setup or installation steps required by the application.

# Example

```yaml
apiVersion: v1
kind: Pod
metadata:
  name: init-test
spec:
  containers:
  - name: nginx
    image: nginx:stable
  initContainers:
  - name: busybox
    image: busybox:stable
    command: ["sh", "-c", "sleep 60"]
```

# Hands-on demonstrations

**1. Create a pod that has an Init container that downloads a file from an external source and saves it to a volume that is shared with the main container.**

```yaml
apiVersion: v1
kind: Pod
metadata:
  name: download-file
spec:
  initContainers:
  - name: init
    image: nginx
    command: ['sh', '-c', 'curl -o /output/dog.jpg https://github.com/JoseAngel1196/ckad-study-notes/blob/master/init_container/images/dog.jpg']
    volumeMounts:
    - name: shared
      mountPath: /output
  containers:
  - name: main
    image: busybox:stable
    command: ["sh", "-c", "while true; do ls /load/dog.jpg; done"]
    volumeMounts:
    - name: shared
      mountPath: /load
  volumes:
  - name: shared
    emptyDir: {}
```