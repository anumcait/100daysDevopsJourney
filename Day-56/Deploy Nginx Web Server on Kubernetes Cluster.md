# Day 56: Deploy Nginx Web Server on Kubernetes Cluster

## Objective
Deploy a highly available, scalable **Nginx static web server** on Kubernetes cluster using Deployment and NodePort Service.

**Requirements:**
- Deployment name: `nginx-deployment`
- Container name: `nginx-container`
- Image: `nginx:latest`
- Replicas: 3
- NodePort Service: `nginx-service` on port `30011`

---

## Step 1: Create Deployment

### Explanation
A **Deployment** manages multiple replicas of a pod and ensures **high availability**. If a pod fails, it automatically spins up a new one.

- **Replicas:** 3 → ensures 3 pods running
- **Container Name:** nginx-container → for identification
- **Image:** nginx:latest → always pulls latest Nginx version

### Deployment YAML
```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: nginx-deployment
spec:
  replicas: 3
  selector:
    matchLabels:
      app: nginx
  template:
    metadata:
      labels:
        app: nginx
    spec:
      containers:
      - name: nginx-container
        image: nginx:latest
        ports:
        - containerPort: 80
```
**Commands**
```bash
kubectl apply -f nginx-deployment.yaml
kubectl get deployments
kubectl get pods -l app=nginx
```
### Step 2: Create NodePort Service

**Explanation**

A **Service** exposes pods to external traffic. **NodePort** allows access via <NodeIP>:<NodePort>.

- **Service** Name: nginx-service
- **Type**: NodePort
- **Port**: 80 (inside cluster)
- **NodePort**: 30011 (external access)

Service YAML
```bash
apiVersion: v1
kind: Service
metadata:
  name: nginx-service
spec:
  selector:
    app: nginx
  type: NodePort
  ports:
    - port: 80
      targetPort: 80
      nodePort: 30011
```
**Commands**
```bash
kubectl apply -f nginx-service.yaml
kubectl get svc nginx-service
```
--- 
**(Optional Steps)**
---

## Step 3: Verify Deployment
```bash
kubectl get pods -l app=nginx
kubectl describe deployment nginx-deployment
kubectl describe svc nginx-service
```
- Access Nginx: http://<NodeIP>:30011

---

## Step 4: Scaling Deployment
```bash
kubectl scale deployment nginx-deployment --replicas=5
kubectl get pods -l app=nginx
```

## Screenshots

<img width="700" height="500" alt="image" src="https://github.com/user-attachments/assets/f20e0d03-1658-44df-a44b-74ded3369604" />

<img width="700" height="500" alt="image" src="https://github.com/user-attachments/assets/f59fdab0-7169-4830-abdc-0dc0f5b59c63" />



