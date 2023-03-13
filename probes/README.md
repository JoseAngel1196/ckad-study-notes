# Probes and Health Checks

Created: March 13, 2023 4:29 PM

# What is a Probe?

Imagine we have a toy car that we really like to play with. But sometimes the car can get stuck or break down, and you're not sure if it's still working properly. That's where probes come in!

In the world of Kubernetes, probes are like little checks that are always running to make sure that the different parts of your application (like the toy car) are still working as they should be. These checks can tell if a part of the application is stuck or has broken down, and can even try to fix the problem automatically.

So basically, probes help keep your application running smoothly and make sure that all the different parts of it are working together correctly.

# Types of Probes

Let's say we have a favorite toy that you like to play with, like a toy car or a doll. Sometimes, you might notice that the toy is not working as well as it should be - maybe the wheels on the car aren't turning, or the arm on the doll is loose.

### Liveness

Liveness probes are like checks that make sure that your toy is still "alive" and working properly. Just like you might check if your toy car is still moving, or if your doll's arm is still attached, liveness probes check if different parts of your application are still working properly. If the probe finds that something is not working right, it can try to fix the problem automatically or alert you so that you can fix it yourself.

### Readiness

On the other hand, readiness probes are like checks that make sure that your toy is "ready" to be played with. Let's say you have a toy castle with a drawbridge that needs to be lowered in order for you to play with the toy knights and princesses inside. Readiness probes check if different parts of your application are ready to be used, just like the drawbridge needs to be lowered before you can play with the toy inside the castle.

So, to sum it up: liveness probes check if your application is still "alive" and working properly, while readiness probes check if your application is "ready" to be used. Both types of probes help make sure that your application is working the way it should be, just like how you check if your toys are working properly before you play with them!

### Startup

Startup probes check container health during startup for slow-starting containers.

# Hands-On Demonstrations

1. Liveness probe

```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  labels:
    app: liveness-probe
  name: liveness-probe
spec:
  replicas: 1
  selector:
    matchLabels:
      app: liveness-probe
  strategy: {}
  template:
    metadata:
      labels:
        app: liveness-probe
    spec:
      containers:
      - image: nginx:stable
        name: nginx
        ports:
        - containerPort: 80
        livenessProbe:
          httpGet:
            path: /
            port: 80
          initialDelaySeconds: 3
          periodSeconds: 3
status: {}
```

1. Readiness probe

```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  labels:
    app: readiness-probe
  name: readiness-probe
spec:
  replicas: 1
  selector:
    matchLabels:
      app: readiness-probe
  strategy: {}
  template:
    metadata:
      labels:
        app: readiness-probe
    spec:
      containers:
      - image: nginx:stable
        name: nginx
        ports:
        - containerPort: 80
        readinessProbe:
          httpGet:
            path: /
            port: 80
          initialDelaySeconds: 3
          periodSeconds: 3
status: {}
```