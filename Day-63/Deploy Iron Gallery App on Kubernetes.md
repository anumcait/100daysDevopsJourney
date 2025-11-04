# Day-63: Deploy Iron Gallery App on Kubernetes

Problem Statement:

There is an iron gallery app that the Nautilus DevOps team was developing. They have recently customized the app and are going to deploy the same on the Kubernetes cluster. Below you can find more details:



Create a namespace iron-namespace-devops

Create a deployment iron-gallery-deployment-devops for iron gallery under the same namespace you created.

:- Labels run should be iron-gallery.

:- Replicas count should be 1.

:- Selector's matchLabels run should be iron-gallery.

:- Template labels run should be iron-gallery under metadata.

:- The container should be named as iron-gallery-container-devops, use kodekloud/irongallery:2.0 image ( use exact image name / tag ).

:- Resources limits for memory should be 100Mi and for CPU should be 50m.

:- First volumeMount name should be config, its mountPath should be /usr/share/nginx/html/data.

:- Second volumeMount name should be images, its mountPath should be /usr/share/nginx/html/uploads.

:- First volume name should be config and give it emptyDir and second volume name should be images, also give it emptyDir.

Create a deployment iron-db-deployment-devops for iron db under the same namespace.

:- Labels db should be mariadb.

:- Replicas count should be 1.

:- Selector's matchLabels db should be mariadb.

:- Template labels db should be mariadb under metadata.

:- The container name should be iron-db-container-devops, use kodekloud/irondb:2.0 image ( use exact image name / tag ).

:- Define environment, set MYSQL_DATABASE its value should be database_web, set MYSQL_ROOT_PASSWORD and MYSQL_PASSWORD value should be with some complex passwords for DB connections, and MYSQL_USER value should be any custom user ( except root ).

:- Volume mount name should be db and its mountPath should be /var/lib/mysql. Volume name should be db and give it an emptyDir.

Create a service for iron db which should be named iron-db-service-devops under the same namespace. Configure spec as selector's db should be mariadb. Protocol should be TCP, port and targetPort should be 3306 and its type should be ClusterIP.

Create a service for iron gallery which should be named iron-gallery-service-devops under the same namespace. Configure spec as selector's run should be iron-gallery. Protocol should be TCP, port and targetPort should be 80, nodePort should be 32678 and its type should be NodePort.


Note:


We don't need to make connection b/w database and front-end now, if the installation page is coming up it should be enough for now.

The kubectl on jump_host has been configured to work with the kubernetes cluster.

----
# 🧩 Day 63 — Deploy Iron Gallery App on Kubernetes  
**KodeKloud 100 Days of DevOps Challenge**

---

## 🎯 Objective

Deploy the **Iron Gallery** front-end application and **Iron DB** backend database on a Kubernetes cluster with proper configuration, namespaces, and services.

---

## 🧱 Task Overview

| Component | Type | Namespace | Image | Ports |
|------------|------|------------|--------|--------|
| Iron Gallery | Deployment + NodePort Service | `iron-namespace-devops` | `kodekloud/irongallery:2.0` | 80 (NodePort: 32678) |
| Iron DB | Deployment + ClusterIP Service | `iron-namespace-devops` | `kodekloud/irondb:2.0` | 3306 |

---

## 🪣 Step 1 — Create Namespace

```bash
kubectl create namespace iron-namespace-devops
```
## ⚙️ Step 2 — Create Iron Gallery Deployment

Create a file named iron-gallery-deployment.yaml:
```bash
apiVersion: apps/v1
kind: Deployment
metadata:
  name: iron-gallery-deployment-devops
  namespace: iron-namespace-devops
spec:
  replicas: 1
  selector:
    matchLabels:
      run: iron-gallery
  template:
    metadata:
      labels:
        run: iron-gallery
    spec:
      containers:
        - name: iron-gallery-container-devops
          image: kodekloud/irongallery:2.0
          resources:
            limits:
              memory: "100Mi"
              cpu: "50m"
          volumeMounts:
            - name: config
              mountPath: /usr/share/nginx/html/data
            - name: images
              mountPath: /usr/share/nginx/html/uploads
      volumes:
        - name: config
          emptyDir: {}
        - name: images
          emptyDir: {}
```

Apply it:
```bash
kubectl apply -f iron-gallery-deployment.yaml
```

## ⚙️ Step 3 — Create Iron DB Deployment

Create a file named iron-db-deployment.yaml:
```bash
apiVersion: apps/v1
kind: Deployment
metadata:
  name: iron-db-deployment-devops
  namespace: iron-namespace-devops
spec:
  replicas: 1
  selector:
    matchLabels:
      db: mariadb
  template:
    metadata:
      labels:
        db: mariadb
    spec:
      containers:
        - name: iron-db-container-devops
          image: kodekloud/irondb:2.0
          env:
            - name: MYSQL_DATABASE
              value: database_web
            - name: MYSQL_ROOT_PASSWORD
              value: "RootP@ssw0rd!"
            - name: MYSQL_USER
              value: "devuser"
            - name: MYSQL_PASSWORD
              value: "UserP@ssw0rd!"
          volumeMounts:
            - name: db
              mountPath: /var/lib/mysql
      volumes:
        - name: db
          emptyDir: {}
```

Apply it:
```bash
kubectl apply -f iron-db-deployment.yaml
```
## ⚙️ Step 4 — Create Services

Create a file named iron-services.yaml:

```bash
apiVersion: v1
kind: Service
metadata:
  name: iron-db-service-devops
  namespace: iron-namespace-devops
spec:
  selector:
    db: mariadb
  ports:
    - protocol: TCP
      port: 3306
      targetPort: 3306
  type: ClusterIP
---
apiVersion: v1
kind: Service
metadata:
  name: iron-gallery-service-devops
  namespace: iron-namespace-devops
spec:
  selector:
    run: iron-gallery
  ports:
    - protocol: TCP
      port: 80
      targetPort: 80
      nodePort: 32678
  type: NodePort
```

Apply it:
```bash
kubectl apply -f iron-services.yaml
```
## ✅ Step 5 — Verify the Deployment

Check all resources in the namespace:
```bash
kubectl get all -n iron-namespace-devops
```

You should see something like:
```
NAME                                               READY   STATUS    RESTARTS   AGE
pod/iron-gallery-deployment-devops-xxxxx           1/1     Running   0          1m
pod/iron-db-deployment-devops-xxxxx                1/1     Running   0          1m

NAME                                TYPE        CLUSTER-IP     PORT(S)          AGE
service/iron-db-service-devops      ClusterIP   10.x.x.x       3306/TCP         1m
service/iron-gallery-service-devops NodePort    10.x.x.x       80:32678/TCP     1m

NAME                                          READY   UP-TO-DATE   AVAILABLE   AGE
deployment.apps/iron-gallery-deployment-devops   1/1     1            1           1m
deployment.apps/iron-db-deployment-devops        1/1     1            1           1m

```
## 🌐 Step 6 — Access the Iron Gallery App

Get your node IP:
```bash
kubectl get nodes -o wide
```

Example output:
```
NAME                      STATUS   ROLES           AGE   VERSION   INTERNAL-IP   EXTERNAL-IP
kodekloud-control-plane   Ready    control-plane   18m   v1.27.16  172.17.0.2    <none>
```

Now open in your browser:

http://172.17.0.2:32678 Click on App icon which is given on top right bar


You should see the Iron Gallery installation page.

---

## 🧹 Step 7 — Clean Up (Optional)

If you want to remove everything:
```bash
kubectl delete namespace iron-namespace-devops
```
---

🏁 Summary
|Resource|	Name|	Type|	Port|	Namespace|
|--------|------|-----|-----|----------|
|Namespace|	iron-namespace-devops|	Namespace|	—|	—|
|Frontend|	iron-gallery-deployment-devops|	Deployment|	80 (NodePort 32678)	|iron-namespace-devops|
|Database|	iron-db-deployment-devops|	Deployment|	3306	|iron-namespace-devops|
|Gallery Service|	iron-gallery-service-devops	|NodePort	80 → 32678|	iron-namespace-devops|
|DB Service|	iron-db-service-devops|	ClusterIP	3306|	iron-namespace-devops|

## 💡 Notes

The front-end (Iron Gallery) and back-end (Iron DB) are not connected yet, as per instructions.

Successful deployment is verified when the installation page of Iron Gallery is accessible via NodePort.

---

## Scrrenshots

<img width="700" height="500" alt="Screenshot 2025-11-04 204307" src="https://github.com/user-attachments/assets/c950166d-2f6b-4fb7-bc9f-4950286cf8e6" />

<img width="700" height="500" alt="Screenshot 2025-11-04 204329" src="https://github.com/user-attachments/assets/440dd248-5afd-4269-afda-4678a6af8243" />

<img width="700" height="500" alt="Screenshot 2025-11-04 204432" src="https://github.com/user-attachments/assets/a34bd71d-382f-4e37-9199-572f503f12c5" />










