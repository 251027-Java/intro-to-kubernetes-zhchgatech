[![Review Assignment Due Date](https://classroom.github.com/assets/deadline-readme-button-22041afd0340ce965d47ae6ef1cefeee28c7c493a6346c4f15d667ab976d596c.svg)](https://classroom.github.com/a/n_BWU6Zt)
# Lab: Kubernetes Deployment

## Goal
Deploy a containerized application to Kubernetes with deployments, services, and scaling.

## Learning Objectives
- Write Kubernetes YAML manifests
- Deploy applications with `kubectl`
- Expose services and test connectivity
- Scale deployments and observe self-healing

## Pre-requisites
- Docker Desktop with Kubernetes enabled, OR Minikube
- kubectl CLI installed
- A Docker image (use `nginx:alpine` if you don't have your own)

## Tasks

### Task 1: Create a Deployment
Create a file `deployment.yaml` with:
- 2 replicas
- Image: `nginx:alpine` (or your own)
- Container port: 80
- Labels: `app: web-app`

**Template**:
```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: # TODO
spec:
  replicas: # TODO
  selector:
    matchLabels:
      app: # TODO
  template:
    metadata:
      labels:
        app: # TODO
    spec:
      containers:
      - name: # TODO
        image: # TODO
        ports:
        - containerPort: # TODO
```

### Task 2: Apply and Verify
```bash
kubectl apply -f deployment.yaml
kubectl get deployments
kubectl get pods
kubectl describe deployment web-app
```

**Questions**:
1. How many pods were created?
2. What is the pod naming pattern?

### Task 3: Create a Service
Create `service.yaml` to expose the deployment:
- Type: `LoadBalancer` (or `NodePort` for Minikube)
- Port: 80
- Target Port: 80

### Task 4: Test Connectivity
```bash
kubectl apply -f service.yaml
kubectl get services

# For LoadBalancer (Docker Desktop)
curl http://localhost:80

# For NodePort (Minikube)
minikube service web-app-service
```

### Task 5: Scale the Deployment
```bash
# Scale to 5 replicas
kubectl scale deployment web-app --replicas=5
kubectl get pods -w
```

**Observe**: Watch new pods come online!

### Task 6: Self-Healing Test
```bash
# Delete a pod
kubectl delete pod <pod-name>

# Watch Kubernetes recreate it
kubectl get pods -w
```

### Task 7: Rolling Update (Bonus)
Update the image to a new version:
```bash
kubectl set image deployment/web-app nginx=nginx:1.25
kubectl rollout status deployment/web-app
```

## Deliverables
1. `deployment.yaml`
2. `service.yaml`
3. Screenshot of running pods
4. Answers to questions

## Starter Code
See `starter_code/` for YAML templates
