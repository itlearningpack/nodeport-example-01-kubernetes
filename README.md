# Run NGINX in kubernetes and exposing it with nodeport service
## Step 1: Create the Nginx Deployment
First, we need to create a deployment for the Nginx web server. A deployment ensures that the specified number of pod replicas are running at all times.

Here’s the deployment configuration:
```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: nginx
  labels:
    nginx: v1.0.0
spec:
  replicas: 2
  selector:
    matchLabels:
      nginx: v1.0.0
  template:
    metadata:
      labels:
        nginx: v1.0.0
    spec:
      containers:
      - name: nginx
        image: nginx:latest
        imagePullPolicy: IfNotPresent
```
Save this YAML configuration to a file named nginx.yaml.

To apply the deployment, run:
```
kubectl apply -f nginx.yaml
```
## Step 2: Create the NodePort Service
Next, we create a NodePort service to expose the Nginx application outside the Kubernetes cluster. This service will listen on a specific port on each node and forward traffic to the Nginx pods.

Here’s the service configuration:
```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: nginx
  labels:
    nginx: v1.0.0
spec:
  replicas: 2
  selector:
    matchLabels:
      nginx: v1.0.0
  template:
    metadata:
      labels:
        nginx: v1.0.0
    spec:
      containers:
      - name: nginx
        image: nginx:latest
        imagePullPolicy: IfNotPresent
---
apiVersion: v1
kind: Service
metadata:
  name: nginxsvc
  labels:
    nginxsvc: v1.0.0
spec:
  selector:
    nginx: v1.0.0
  ports:
  - protocol: TCP
    port: 80
    targetPort: 80
    nodePort: 30001
  type: NodePort
```
```
kubectl apply -f nginx.yaml
```
