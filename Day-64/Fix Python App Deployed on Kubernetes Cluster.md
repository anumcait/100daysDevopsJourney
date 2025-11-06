# 🐍 Day 64 — Fix Python App Deployed on Kubernetes Cluster  

Problem statement:
One of the DevOps engineers was trying to deploy a python app on Kubernetes cluster. Unfortunately, due to some mis-configuration, the application is not coming up. Please take a look into it and fix the issues. Application should be accessible on the specified nodePort.

The deployment name is python-deployment-datacenter, its using poroko/flask-demo-appimage. The deployment and service of this app is already deployed.

nodePort should be 32345 and targetPort should be python flask app's default port.

Note: The kubectl on jump_host has been configured to work with the kubernetes cluster.

---

## 🧭 Objective

A Python Flask application has been deployed on a Kubernetes cluster but isn’t coming up due to misconfiguration.  
Your task is to identify and fix the issue so that the application becomes accessible on the specified **NodePort**.

---

## 📋 Given Information

| Resource | Details |
|-----------|----------|
| **Deployment Name** | `python-deployment-datacenter` |
| **Incorrect Image** | `poroko/flask-app-demo` |
| **Correct Image** | `poroko/flask-demo-app` |
| **Flask Port (targetPort)** | `5000` |
| **NodePort** | `32345` |

> 🧠 **Note:** `kubectl` on the `jump_host` has already been configured to work with the Kubernetes cluster.

---

## ⚙️ Step 1 — Check Existing Pods

List all pods using the app label:

```bash
kubectl get pods -l app=python_app
```
You might find the pod in an error state such as ErrImagePull or ImagePullBackOff.

---

## 🔍 Step 2 — Describe the Pod

Inspect the pod to identify the root cause:
```bash
kubectl describe pod python-deployment-datacenter-<pod-name>
```

You’ll likely see something like this:

Failed to pull image "poroko/flask-app-demo": pull access denied, repository does not exist

✅ Root Cause:
The image name is incorrect. The correct image is poroko/flask-demo-app.

---

## 🧩 Step 3 — Fix the Deployment

Edit the deployment to correct the image name:

kubectl edit deployment python-deployment-datacenter


Locate the container spec and modify it as follows:
```bash
containers:
- name: python-container-datacenter
  image: poroko/flask-demo-app
  ports:
  - containerPort: 5000
```

Save and exit (:wq if using vi).

---

🚀 Step 4 — Verify Rollout and Pod Status

Check rollout status:
```bash
kubectl rollout status deployment python-deployment-datacenter
```

Monitor pod status:
```bash
kubectl get pods -w
```

You should now see the pod running successfully:
```
NAME                                            READY   STATUS    RESTARTS   AGE
python-deployment-datacenter-xxxxxxx-xxxxx      1/1     Running   0          20s
```
---

## 🌐 Step 5 — Verify the Service Configuration

### Check the service definition:
```bash
kubectl get svc python-deployment-datacenter -o yaml
```

Ensure the following configuration:
```
spec:
  type: NodePort
  ports:
  - port: 5000
    targetPort: 5000
    nodePort: 32345
```

If needed, edit it:

kubectl edit svc python-deployment-datacenter

---

## 🧪 Step 6 — Test App Accessibility

Get the node’s IP:
```bash
kubectl get nodes -o wide
```

Test the application from the jump host:
```bash
curl http://<node-ip>:32345
```

✅ Expected Output:
You should see a response from the Flask app, such as “Hello World”.

<img width="500" height="300" alt="image" src="https://github.com/user-attachments/assets/85138fdd-9ec5-4df1-9716-b5627fea2602" />

<img width="500" height="300" alt="image" src="https://github.com/user-attachments/assets/c8fdf742-b5a3-49b6-a9ea-507a1f80cc0a" />
<img width="500" height="300" alt="image" src="https://github.com/user-attachments/assets/d3147057-83e4-4bb5-896f-a40c33eca42d" />

---







