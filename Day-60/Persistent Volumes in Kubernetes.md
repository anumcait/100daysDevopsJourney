# Day 60: Persistent Volumes in Kubernetes

Problem Statement:
The Nautilus DevOps team is working on a Kubernetes template to deploy a web application on the cluster. There are some requirements to create/use persistent volumes to store the application code, and the template needs to be designed accordingly. Please find more details below:


Create a PersistentVolume named as pv-datacenter. Configure the spec as storage class should be manual, set capacity to 4Gi, set access mode to ReadWriteOnce, volume type should be hostPath and set path to /mnt/data (this directory is already created, you might not be able to access it directly, so you need not to worry about it).

Create a PersistentVolumeClaim named as pvc-datacenter. Configure the spec as storage class should be manual, request 2Gi of the storage, set access mode to ReadWriteOnce.

Create a pod named as pod-datacenter, mount the persistent volume you created with claim name pvc-datacenter at document root of the web server, the container within the pod should be named as container-datacenter using image nginx with latest tag only (remember to mention the tag i.e nginx:latest).

Create a node port type service named web-datacenter using node port 30008 to expose the web server running within the pod.

Note: The kubectl utility on jump_host has been configured to work with the kubernetes cluster.

---

## 🎯 Challenge Objective

The Nautilus DevOps team needs to deploy a web application in Kubernetes that uses persistent storage for application data.
We’ll configure:

1. PersistentVolume (PV)
2. PersistentVolumeClaim (PVC)
3. Pod mounting the PVC
4. NodePort Service to expose the web app

---

## 🧩 Step 1: Create a PersistentVolume — pv-datacenter

### 🧠 What

A **PersistentVolume (PV)** is a piece of storage in the cluster provisioned by an administrator.
It defines where and how much data storage is available for pods.

### 💡 Why

Pods are ephemeral — when a pod restarts or moves, any data stored inside it is lost.
A PV ensures that application data **persists** even if the pod is recreated or rescheduled.

### ⚙️ How

We’ll create a PV that:

- Uses hostPath at /mnt/data
- Has 4Gi capacity
- Access mode: ReadWriteOnce (only one node can write at a time)
- Storage class: manual

pv.yaml
```yaml
apiVersion: v1
kind: PersistentVolume
metadata:
  name: pv-datacenter
spec:
  storageClassName: manual
  capacity:
    storage: 4Gi
  accessModes:
    - ReadWriteOnce
  hostPath:
    path: /mnt/data
```
Apply and verify
```bash
kubectl apply -f pv.yaml
kubectl get pv
```

✅ Expected Output
```pgsql
NAME            CAPACITY   ACCESS MODES   RECLAIM POLICY   STATUS      CLAIM   STORAGECLASS   AGE
pv-datacenter   4Gi        RWO            Retain           Available           manual         2s
```

---

## 📦 Step 2: Create a PersistentVolumeClaim — pvc-datacenter

### 🧠 What

A PersistentVolumeClaim (PVC) is a user request for storage.
It specifies how much storage an application needs and the access mode required.

### 💡 Why

PVCs provide an abstraction layer — developers request storage without needing to know the details of the underlying physical storage.

### ⚙️ How

We’ll create a PVC that:

- Requests 2Gi storage
- Uses the manual storage class
- Access mode ReadWriteOnce

pvc.yaml
```yaml
apiVersion: v1
kind: PersistentVolumeClaim
metadata:
  name: pvc-datacenter
spec:
  storageClassName: manual
  accessModes:
    - ReadWriteOnce
  resources:
    requests:
      storage: 2Gi
```

**Apply and verify**

```bash
kubectl apply -f pvc.yaml
kubectl get pvc
```

**✅ Expected Output**

```pgsql
NAME             STATUS   VOLUME          CAPACITY   ACCESS MODES   STORAGECLASS   AGE
pvc-datacenter   Bound    pv-datacenter   4Gi        RWO            manual         5s
```

(STATUS: Bound means the claim is successfully matched to the PV.)

---

## 🧱 Step 3: Create a Pod — pod-datacenter

### 🧠 What

A Pod is the smallest deployable unit in Kubernetes — it runs one or more containers.

### 💡 Why

Our application (Nginx web server) needs access to persistent data, so we’ll mount the PVC inside the pod at /usr/share/nginx/html — the default Nginx document root.

### ⚙️ How

We’ll:

- Use the nginx:latest image
- Name the container container-datacenter
- Mount the PVC at /usr/share/nginx/html
- Label the pod so the service can find it

**pod.yaml**

```yaml
apiVersion: v1
kind: Pod
metadata:
  name: pod-datacenter
  labels:
    app: web-datacenter
spec:
  containers:
    - name: container-datacenter
      image: nginx:latest
      ports:
        - containerPort: 80
      volumeMounts:
        - name: datacenter-storage
          mountPath: /usr/share/nginx/html
  volumes:
    - name: datacenter-storage
      persistentVolumeClaim:
        claimName: pvc-datacenter
```

**Apply and verify**
```bash
kubectl apply -f pod.yaml
kubectl get pods
kubectl describe pod pod-datacenter
```

**✅ Expected Output**

```pgsql
NAME             READY   STATUS    RESTARTS   AGE
pod-datacenter   1/1     Running   0          10s
```

## 🌐 Step 4: Create a NodePort Service — web-datacenter

### 🧠 What

A **Service** provides stable network access to pods.
A **NodePort** service exposes the application outside the cluster on a specific port of each node.

## 💡 Why

Without a service, the Nginx pod is only accessible inside the cluster.
We use NodePort to allow external (browser/curl) access on port 30008.

## ⚙️ How

**service.yaml**
```yaml
apiVersion: v1
kind: Service
metadata:
  name: web-datacenter
spec:
  type: NodePort
  selector:
    app: web-datacenter
  ports:
    - port: 80
      targetPort: 80
      nodePort: 30008
```

**Apply and verify**
```bash
kubectl apply -f service.yaml
kubectl get svc
```

**✅ Expected Output**

```pgsql
NAME             TYPE       CLUSTER-IP      EXTERNAL-IP   PORT(S)        AGE
web-datacenter   NodePort   10.96.245.10    <none>        80:30008/TCP   5s
```

## 🔍 Step 5: Test the Setup

If you have node access:

```bash
curl http://<node-ip>:30008
```

✅ You should see the **Nginx default welcome page.**

Check that the volume is mounted:
```bash
kubectl exec -it pod-datacenter -- df -h /usr/share/nginx/html
```
---

## Screenshots
<img width="700" height="500" alt="image" src="https://github.com/user-attachments/assets/59661f0d-2c40-4cfd-b214-bb42187cd294" />

<img width="700" height="500" alt="image" src="https://github.com/user-attachments/assets/dae9d318-630c-4a09-836c-e3ca5b701b00" />

<img width="700" height="500" alt="image" src="https://github.com/user-attachments/assets/ca375b94-6f9e-4e7e-ace9-4244d6927f9d" />





