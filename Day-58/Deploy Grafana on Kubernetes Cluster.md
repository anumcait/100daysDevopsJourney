# 🚀 Day 58 - Deploy Grafana on Kubernetes Cluster

### Problem Statement

The Nautilus DevOps teams is planning to set up a Grafana tool to collect and analyze analytics from some applications. They are planning to deploy it on Kubernetes cluster. Below you can find more details.



1.) Create a deployment named grafana-deployment-xfusion using any grafana image for Grafana app. Set other parameters as per your choice.


2.) Create NodePort type service with nodePort 32000 to expose the app.


You need not to make any configuration changes inside the Grafana app once deployed, just make sure you are able to access the Grafana login page.


Note: The kubectl on jump_host has been configured to work with kubernetes cluster.

---


## 🧠 Challenge Description
The Nautilus DevOps team plans to set up **Grafana** to collect and analyze metrics from various applications.  
They want to deploy it on a **Kubernetes cluster** and make it accessible via a **NodePort service**.

---

## 🎯 Task Objectives

1. Create a **Deployment** named `grafana-deployment-xfusion` using any Grafana image.  
2. Expose it with a **NodePort Service** on port `32000`.  
3. Verify that the **Grafana login page** is accessible (no internal configuration changes needed).

> 📝 Note: `kubectl` is already configured to access the cluster on the jump host.

---

## ⚙️ Step 1 — Create the Deployment

Create a deployment manifest file named **`grafana-deployment.yaml`**:

```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: grafana-deployment-xfusion
  labels:
    app: grafana
spec:
  replicas: 1
  selector:
    matchLabels:
      app: grafana
  template:
    metadata:
      labels:
        app: grafana
    spec:
      containers:
      - name: grafana
        image: grafana/grafana:latest
        ports:
        - containerPort: 3000
```
Apply the deployment:
```bash
kubectl apply -f grafana-deployment.yaml
```
Verify the deployment and pod:
```bash
kubectl get deployments
kubectl get pods
```
Expected output:
```pgsql
NAME                         READY   UP-TO-DATE   AVAILABLE   AGE
grafana-deployment-xfusion   1/1     1            1           1m
```
---
## ⚙️ Step 2 — Create a NodePort Service

Create the service manifest grafana-service.yaml:
```bash
apiVersion: v1
kind: Service
metadata:
  name: grafana-service-xfusion
spec:
  type: NodePort
  selector:
    app: grafana
  ports:
  - port: 3000
    targetPort: 3000
    nodePort: 32000
```
Apply the service:
```bash
kubectl apply -f grafana-service.yaml
```

Check the service:
```bash
kubectl get svc
```
Expected output:
```bash
NAME                      TYPE       CLUSTER-IP      EXTERNAL-IP   PORT(S)          AGE
grafana-service-xfusion   NodePort   10.96.197.204   <none>        3000:32000/TCP   2m
```
---
## 🧩 Step 3 — Access Grafana

Find your Node IP:
```bash
kubectl get nodes -o wide
```
Example output:
```pgsql
NAME     STATUS   ROLES    AGE   VERSION   INTERNAL-IP     OS-IMAGE
node01   Ready    <none>   3d    v1.29.0   172.16.238.10   Ubuntu 22.04 LTS
```

Now access Grafana using NodePort:
```bash
curl http://<INTERNAL-IP>:32000
```

Or, if running locally (e.g., Minikube), use:
```bash
curl http://localhost:32000
```

Expected result: You’ll receive the HTML for the Grafana login page.
(Default credentials: admin / admin)

## 🧰 Alternative Access (Port Forwarding) / Troubleshooting 

If NodePort is inaccessible from your jump host, you can use:
```bash
kubectl port-forward svc/grafana-service-xfusion 3000:3000
```

Then open in browser or check with curl:
```bash
curl http://localhost:3000
```
<img width="700" height="500" alt="Screenshot 2025-10-29 210729" src="https://github.com/user-attachments/assets/3d2f87e0-9a76-4249-bc19-dd99966875fc" />

