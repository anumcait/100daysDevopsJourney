# Day 49: Deploy Applications with Kubernetes Deployments
The Nautilus DevOps team is delving into Kubernetes for app management. One team member needs to create a deployment following these details:


Create a deployment named nginx to deploy the application nginx using the image nginx:latest (ensure to specify the tag)

Note: The kubectl utility on jump_host is set up to interact with the Kubernetes cluster.

## 🎯 Objectives

- Create a deployment named nginx
- Use the Docker image nginx:latest (explicitly specify the tag)
- Ensure the deployment is properly created and running in the cluster

## ⚙️ Environment

kubectl is pre-configured on the jump_host to interact with the Kubernetes cluster.

## 🛠️ Steps to Complete the Task

Create the deployment using kubectl:
```
kubectl create deployment nginx --image=nginx:latest
```

Verify the deployment:
```
kubectl get deployments
```

Optional: Describe the deployment for more details:
```
kubectl describe deployment nginx
```
### ✅ Validation

To confirm the deployment was successful:

The deployment nginx should appear in the list from kubectl get deployments.

At least one pod should be running using the nginx:latest image.

### 📌 Notes

Always specify the image tag (:latest) even if it's the default.

This setup does not expose the deployment as a service; it's purely for running the application inside the cluster.

📷 Screenshots

<img width="700" height="500" alt="image" src="https://github.com/user-attachments/assets/6a82de43-8c95-405e-b45f-5c3fc93d6dea" />

<img width="700" height="500" alt="image" src="https://github.com/user-attachments/assets/e95a0a26-ae71-432a-9657-a85568160b2b" />

