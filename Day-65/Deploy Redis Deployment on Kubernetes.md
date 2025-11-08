# 🧩 Day 65 – Deploy Redis Deployment on Kubernetes
Problem Statement:

The Nautilus application development team observed some performance issues with one of the application that is deployed in Kubernetes cluster. After looking into number of factors, the team has suggested to use some in-memory caching utility for DB service. After number of discussions, they have decided to use Redis. Initially they would like to deploy Redis on kubernetes cluster for testing and later they will move it to production. Please find below more details about the task:


Create a redis deployment with following parameters:

Create a config map called my-redis-config having maxmemory 2mb in redis-config.

Name of the deployment should be redis-deployment, it should use
redis:alpine image and container name should be redis-container. Also make sure it has only 1 replica.

The container should request for 1 CPU.

Mount 2 volumes:

a. An Empty directory volume called data at path /redis-master-data.

b. A configmap volume called redis-config at path /redis-master.

c. The container should expose the port 6379.

Finally, redis-deployment should be in an up and running state.
Note: The kubectl utility on jump_host has been configured to work with the kubernetes cluster.

---
## Task Description

The Nautilus DevOps team needs to deploy a Redis service in the Kubernetes cluster for testing.
This setup will later help improve database performance using in-memory caching.

## ⚙️ Requirements

- ConfigMap named my-redis-config
  - Contains: maxmemory 2mb in key redis-config
- Deployment named redis-deployment
   - Image: redis:alpine
   - Container name: redis-container
   - Replicas: 1
   - CPU request: 1
   - Port: 6379
   - Volume mounts:
- emptyDir → /redis-master-data
- ConfigMap (my-redis-config) → /redis-master

---

## 🧾 Step-by-Step Implementation
**Step 1: Create the ConfigMap**

Create a file named redis-configmap.yaml.
```bash
apiVersion: v1
kind: ConfigMap
metadata:
  name: my-redis-config
data:
  redis-config: |
    maxmemory 2mb
```

Apply it:
```bash
kubectl apply -f redis-configmap.yaml
```

Verify:
```bash
kubectl get configmap my-redis-config -o yaml
```
**Step 2: Create the Redis Deployment**

Create a file named redis-deployment.yaml.
```bash
apiVersion: apps/v1
kind: Deployment
metadata:
  name: redis-deployment
spec:
  replicas: 1
  selector:
    matchLabels:
      app: redis
  template:
    metadata:
      labels:
        app: redis
    spec:
      containers:
      - name: redis-container
        image: redis:alpine
        resources:
          requests:
            cpu: "1"
        ports:
        - containerPort: 6379
        volumeMounts:
        - name: data
          mountPath: /redis-master-data
        - name: redis-config
          mountPath: /redis-master/redis.conf
          subPath: redis-config
      volumes:
      - name: data
        emptyDir: {}
      - name: redis-config
        configMap:
          name: my-redis-config
```

Apply the deployment:
```bash
kubectl apply -f redis-deployment.yaml
```
**Step 3: Verify the Deployment**

Check deployment status:
```bash
kubectl get deployments
```

Check pods:
```bash
kubectl get pods
```

Verify logs:
```bash
kubectl logs -l app=redis
```
**Step 4: Verify Volumes and Config**

Access the Redis pod:
```bash
kubectl exec -it $(kubectl get pod -l app=redis -o jsonpath='{.items[0].metadata.name}') -- sh
```

Check mounted directories:
```bash`
ls /redis-master-data
cat /redis-master/redis.conf
```

You should see:

maxmemory 2mb

**Step 5: Cleanup (Optional)**

If you want to remove the Redis setup:
```bash
kubectl delete deployment redis-deployment
kubectl delete configmap my-redis-config
```

<img width="700" height="500" alt="Screenshot 2025-11-08 220905" src="https://github.com/user-attachments/assets/41808cc5-1eeb-484f-8769-86244309eb5f" />






