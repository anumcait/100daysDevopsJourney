# Day 48: Deploy Pods in Kubernetes Cluster - KodeKloud 100 Days of DevOps

## 🧠 Task Summary

The Nautilus DevOps team is working with Kubernetes to manage applications. One team member has been tasked with creating a pod with the following specifications:

### ✅ Requirements:
- **Pod Name**: `pod-httpd`
- **Image**: `httpd:latest`
- **Container Name**: `httpd-container`
- **Label**: `app=httpd_app`

---

## 🛠️ Solution

### Option 1: Using a YAML file (Recommended)

1. Create a file named `pod-httpd.yaml` with the following content:

```yaml
apiVersion: v1
kind: Pod
metadata:
  name: pod-httpd
  labels:
    app: httpd_app
spec:
  containers:
    - name: httpd-container
      image: httpd:latest
```
2. Apply the YAML file:

```
kubectl apply -f pod-httpd.yaml
```
### Option 2: Using kubectl run Command
```bash
kubectl run pod-httpd \
  --image=httpd:latest \
  --restart=Never \
  --labels=app=httpd_app \
  --overrides='
{
  "apiVersion": "v1",
  "spec": {
    "containers": [{
      "name": "httpd-container",
      "image": "httpd:latest"
    }]
  }
}'
```
### ✅ Verification Commands

Check if the pod is running:
```
kubectl get pods
```
Describe the pod for more details:
```
kubectl describe pod pod-httpd
```
Check pod labels:
```
kubectl get pod pod-httpd --show-labels
```

🧹 Cleanup (if needed)
```
kubectl delete pod pod-httpd
```

### Screenshots
<img width="1050" height="522" alt="image" src="https://github.com/user-attachments/assets/3456c940-d1cf-4080-8664-bcc57f4b8a76" />

<img width="1050" height="528" alt="image" src="https://github.com/user-attachments/assets/5fb68275-6469-4478-8bc7-11506db9a41a" />




