# Day-51 : Execute Rolling Updates in Kubernetes

An application currently running on the Kubernetes cluster employs the nginx web server. The Nautilus application development team has introduced some recent changes that need deployment. They've crafted an image nginx:1.19 with the latest updates.

Execute a rolling update for this application, integrating the nginx:1.19 image. The deployment is named nginx-deployment.

Ensure all pods are operational post-update.

Note: The kubectl utility on jump_host is set up to operate with the Kubernetes cluster


## 📝 Task
Perform a rolling update for an existing Kubernetes deployment `nginx-deployment`, updating the container image from `nginx:1.16` to `nginx:1.19`.

---

## 📦 Current Deployment Details

- **Deployment Name**: `nginx-deployment`
- **Current Image**: `nginx:1.16`
- **Target Image**: `nginx:1.19`
- **Container Name**: `nginx-container`
- **Replicas**: 3
- **Strategy**: RollingUpdate (25% max unavailable, 25% max surge)

---

## ✅ Steps to Execute

### 1. Check Existing Deployment

```bash
kubectl describe deployment nginx-deployment
```
## 2. Perform Rolling Update
```
kubectl set image deployment/nginx-deployment nginx-container=nginx:1.19
```
## 3. Monitor Rollout
```
kubectl rollout status deployment/nginx-deployment
```

## Expected Output:

deployment "nginx-deployment" successfully rolled out

## 4. Verify Pods Are Running
```
kubectl get pods
```
Ensure all pods are in Running state.

## 5. Confirm Image Update
kubectl describe deployment nginx-deployment | grep Image


Expected Output:

Image: nginx:1.19

## ✅ Success Criteria

- All 3 pods updated to nginx:1.19
- Deployment rollout completed successfully
- All pods in Running state

## Screenshots
<img width="700" height="500" alt="Screenshot 2025-10-19 200142" src="https://github.com/user-attachments/assets/1f0d3258-b89e-4def-a9d9-bf81b5dacd1d" />

<img width="700" height="500" alt="Screenshot 2025-10-19 200212" src="https://github.com/user-attachments/assets/61b01fad-0d6e-482b-87eb-ab75a31a14e3" />

