# 📘 Day 67 – Deploy Guestbook App on Kubernetes

Problem Statement

The Nautilus Application development team has finished development of one of the applications and it is ready for deployment. It is a guestbook application that will be used to manage entries for guests/visitors. As per discussion with the DevOps team, they have finalized the infrastructure that will be deployed on Kubernetes cluster. Below you can find more details about it.


BACK-END TIER

Create a deployment named redis-master for Redis master.

a.) Replicas count should be 1.

b.) Container name should be master-redis-xfusion and it should use image redis.

c.) Request resources as CPU should be 100m and Memory should be 100Mi.

d.) Container port should be redis default port i.e 6379.

Create a service named redis-master for Redis master. Port and targetPort should be Redis default port i.e 6379.

Create another deployment named redis-slave for Redis slave.

a.) Replicas count should be 2.

b.) Container name should be slave-redis-xfusion and it should use gcr.io/google_samples/gb-redisslave:v3 image.

c.) Requests resources as CPU should be 100m and Memory should be 100Mi.

d.) Define an environment variable named GET_HOSTS_FROM and its value should be dns.

e.) Container port should be Redis default port i.e 6379.

Create another service named redis-slave. It should use Redis default port i.e 6379.

FRONT END TIER

Create a deployment named frontend.

a.) Replicas count should be 3.

b.) Container name should be php-redis-xfusion and it should use gcr.io/google-samples/gb-frontend@sha256:a908df8486ff66f2c4daa0d3d8a2fa09846a1fc8efd65649c0109695c7c5cbff image.

c.) Request resources as CPU should be 100m and Memory should be 100Mi.

d.) Define an environment variable named as GET_HOSTS_FROM and its value should be dns.

e.) Container port should be 80.

Create a service named frontend. Its type should be NodePort, port should be 80 and its nodePort should be 30009.

Finally, you can check the guestbook app by clicking on App button.

You can use any labels as per your choice.

Note: The kubectl utility on jump_host has been configured to work with the kubernetes cluster.

---

🎯 Objective

Deploy a Guestbook application with a Redis backend on a Kubernetes cluster.
The app consists of:

- Redis Master (Backend tier)
- Redis Slaves (Backend tier)
- Frontend (PHP) (Frontend tier)

### 🧰 Prerequisites

- Access to a running Kubernetes cluster
- kubectl configured and working
- Basic understanding of Deployments and Services

### ⚙️ Step 1: Redis Master Deployment and Service

File: redis-master.yaml
```
apiVersion: apps/v1
kind: Deployment
metadata:
  name: redis-master
  labels:
    app: guestbook
    tier: backend
    role: master
spec:
  replicas: 1
  selector:
    matchLabels:
      app: guestbook
      role: master
  template:
    metadata:
      labels:
        app: guestbook
        role: master
    spec:
      containers:
        - name: master-redis-xfusion
          image: redis
          resources:
            requests:
              cpu: "100m"
              memory: "100Mi"
          ports:
            - containerPort: 6379
---
apiVersion: v1
kind: Service
metadata:
  name: redis-master
  labels:
    app: guestbook
    role: master
spec:
  ports:
    - port: 6379
      targetPort: 6379
  selector:
    app: guestbook
    role: master
```
---

### ⚙️ Step 2: Redis Slave Deployment and Service

File: redis-slave.yaml
```
apiVersion: apps/v1
kind: Deployment
metadata:
  name: redis-slave
  labels:
    app: guestbook
    tier: backend
    role: slave
spec:
  replicas: 2
  selector:
    matchLabels:
      app: guestbook
      role: slave
  template:
    metadata:
      labels:
        app: guestbook
        role: slave
    spec:
      containers:
        - name: slave-redis-xfusion
          image: gcr.io/google_samples/gb-redisslave:v3
          resources:
            requests:
              cpu: "100m"
              memory: "100Mi"
          env:
            - name: GET_HOSTS_FROM
              value: "dns"
          ports:
            - containerPort: 6379
---
apiVersion: v1
kind: Service
metadata:
  name: redis-slave
  labels:
    app: guestbook
    role: slave
spec:
  ports:
    - port: 6379
      targetPort: 6379
  selector:
    app: guestbook
    role: slave
```
---

### ⚙️ Step 3: Frontend Deployment and Service

File: frontend.yaml
```
apiVersion: apps/v1
kind: Deployment
metadata:
  name: frontend
  labels:
    app: guestbook
    tier: frontend
spec:
  replicas: 3
  selector:
    matchLabels:
      app: guestbook
      tier: frontend
  template:
    metadata:
      labels:
        app: guestbook
        tier: frontend
    spec:
      containers:
        - name: php-redis-xfusion
          image: gcr.io/google-samples/gb-frontend:v5
          resources:
            requests:
              cpu: "100m"
              memory: "100Mi"
          env:
            - name: GET_HOSTS_FROM
              value: "dns"
          ports:
            - containerPort: 80
---
apiVersion: v1
kind: Service
metadata:
  name: frontend
  labels:
    app: guestbook
    tier: frontend
spec:
  type: NodePort
  ports:
    - port: 80
      targetPort: 80
      nodePort: 30009
  selector:
    app: guestbook
    tier: frontend
```
### 🚀 Step 4: Apply All Configurations

Run the following commands on your terminal:
```
kubectl apply -f redis-master.yaml
kubectl apply -f redis-slave.yaml
kubectl apply -f frontend.yaml
```

### 🧩 Step 5: Verify Deployments and Services

Check that all Deployments and Services are up:
```
kubectl get deployments
kubectl get pods
kubectl get svc
```

You should see output similar to:
```
NAME           READY   UP-TO-DATE   AVAILABLE   AGE
frontend       3/3     3            3           1m
redis-master   1/1     1            1           1m
redis-slave    2/2     2            2           1m

NAME           TYPE        CLUSTER-IP     EXTERNAL-IP   PORT(S)        AGE
frontend       NodePort    10.0.0.10      <none>        80:30009/TCP   1m
redis-master   ClusterIP   10.0.0.11      <none>        6379/TCP       1m
redis-slave    ClusterIP   10.0.0.12      <none>        6379/TCP       1m
```

### 🌐 Step 6: Access the Guestbook App

Find the Node’s IP address:

| kubectl get nodes -o wide


Then open the app in your browser:

| http://<Node-IP>:30009


You should see the Guestbook App running successfully 🎉

### 🧹 Step 7: Cleanup (Optional)

When you’re done testing:
```bash
kubectl delete -f redis-master.yaml
kubectl delete -f redis-slave.yaml
kubectl delete -f frontend.yaml
```
---

### 🏁 Summary
|Component|	Replicas|	Image|	Service| Type|	Port|
|---------|---------|------|---------|-----|------|
|Redis Master|	1|	redis|	ClusterIP|	6379|
|Redis Slave|	2|	gcr.io/google_samples/gb-redisslave:v3|	ClusterIP|	6379|
|Frontend|	3|	gcr.io/google-samples/gb-frontend:v5|	NodePort|	30009|

---

### 💡 Notes

- You can modify replica counts or resource requests as needed.
- All services are internal (ClusterIP) except the frontend (NodePort).
- The frontend connects to Redis via DNS (service names).



