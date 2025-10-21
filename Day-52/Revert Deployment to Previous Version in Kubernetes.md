# Day 52: Revert Deployment to Previous Version in Kubernetes

### Problem Statement:

Earlier today, the Nautilus DevOps team deployed a new release for an application. However, a customer has reported a bug related to this recent release. Consequently, the team aims to revert to the previous version.

There exists a deployment named nginx-deployment; initiate a rollback to the previous revision.

Note: The kubectl utility on jump_host is configured to interact with the Kubernetes cluster.

## 🎯 Objective

Revert the `nginx-deployment` to the previous working version due to a bug in the latest release.

---

## 📋 Prerequisites

- You are logged into the `jump_host` as `thor`.
- `kubectl` is configured to interact with the target Kubernetes cluster.

---

## 🪜 Step-by-Step Guide

### 🔍 Step 1: Check Deployment Revision History

Run the following command to inspect the deployment's revision history:

```bash
kubectl rollout history deployment nginx-deployment
```
You should see output similar to:

deployment.apps/nginx-deployment 

| REVISION | CHANGE-CAUSE                                                                 |
|----------|------------------------------------------------------------------------------|
| 1        | <none>                                                                       |
| 2        | kubectl set image deployment nginx-deployment nginx-container=nginx:stable   |

## ⏪ Step 2: Roll Back to the Previous Revision

Since revision 2 introduced the bug, roll back to revision 1 (previous version):
```bash
kubectl rollout undo deployment nginx-deployment
```

This will revert the deployment to its previous state (revision 1).

## 📈 Step 3: Verify the Rollback Status

Check if the rollback completed successfully:
```bash
kubectl rollout status deployment nginx-deployment
```

You should see a message like:

deployment "nginx-deployment" successfully rolled out

## 🔍 Step 4: Confirm the Current Deployment Details (Optional)

To verify the current container image and configuration:
```bash
kubectl describe deployment nginx-deployment
```
Look for the Image: line under the Containers: section to confirm it's no longer using nginx:stable.

## ✅ Outcome

The nginx-deployment is now reverted to the previous stable version, restoring application functionality for the customer.

## screenshot

<img width="700" height="500" alt="image" src="https://github.com/user-attachments/assets/5ec51326-683e-4f53-9dd0-4fcb24e27907" />



