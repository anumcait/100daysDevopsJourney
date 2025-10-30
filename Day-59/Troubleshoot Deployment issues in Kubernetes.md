# Day 59: Troubleshoot Deployment issues in Kubernetes - Redis Deployment

## Scenario
The Nautilus DevOps team deployed a Redis app on a Kubernetes cluster. After some changes, the app went down. The deployment name is `redis-deployment`. Pods are not in running state.

We need to identify and fix the issue.

---

## Step 1: Check the pod status
```bash
kubectl get pods
kubectl describe pod redis-deployment-<pod-id>
```
**Observation:**
The pod was Pending and ContainerCreating. The describe output showed:
```vbnet
FailedMount: configmap "redis-cofig" not found
```
---
## Step 2: Verify the ConfigMap
```bash
kubectl get configmap redis-cofig
```
Output:
```psql
Error from server (NotFound): configmaps "redis-cofig" not found
```
Analysis: The deployment is referencing a ConfigMap with a typo (redis-cofig).

---

## Step 3: Check Deployment YAML for ConfigMap
```bash
kubectl get deployment redis-deployment -o yaml | grep -A3 configMap
```
Observation:
```yaml
volumes:
  - emptyDir: {}
    name: data
  - configMap:
      defaultMode: 420
      name: redis-cofig
    name: config
```
---

## Step 4: Fix the ConfigMap reference
```bash
kubectl edit deployment redis-deployment
```

Change:
```bash
name: redis-cofig
```
to:
```bash
name: redis-config
```

Save and exit.

## Step 5: Ensure the correct ConfigMap exists
```bash
kubectl get configmap redis-config
```

If not found:
```bash
kubectl create configmap redis-config --from-literal=redis.conf="maxmemory 2mb"
```
In this case, redis-config already existed.

## Step 6: Identify pod issues after ConfigMap fix
```bash
kubectl get pods -o wide
```

Observation:
- Old pod: ContainerCreating
- New pod: ImagePullBackOff

Analysis: Image name has a typo: redis:alpin → should be redis:alpine.

---

## Step 7: Fix the image
```bash
kubectl set image deployment/redis-deployment redis-container=redis:alpine
```
## Step 8: Check rollout status
```bash
kubectl rollout status deployment redis-deployment
```

Wait until it shows:

- deployment "redis-deployment" successfully rolled out

---

## Step 9: Cleanup old pods if stuck
```bash
kubectl get pods
kubectl delete pod <old-pod-name> --force --grace-period=0
```

---

## Step 10: Verify the deployment
```bash
kubectl get pods
kubectl logs -l app=redis
```

Expected output:

1:C ... # Server initialized
1:M ... * Ready to accept connections

<img width="1829" height="850" alt="Screenshot 2025-10-30 220359" src="https://github.com/user-attachments/assets/f63141a8-21d0-47a2-99cb-eb8c417dc985" />


