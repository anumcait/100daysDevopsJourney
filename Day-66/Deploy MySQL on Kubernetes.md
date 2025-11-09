# 🗓️ Day 66: Deploy MySQL on Kubernetes
## 🧩 Challenge Objective

A new MySQL server needs to be deployed on Kubernetes cluster. The Nautilus DevOps team was working on to gather the requirements. Recently they were able to finalize the requirements and shared them with the team members to start working on it. Below you can find the details:

1.) Create a PersistentVolume mysql-pv, its capacity should be 250Mi, set other parameters as per your preference.

2.) Create a PersistentVolumeClaim to request this PersistentVolume storage. Name it as mysql-pv-claim and request a 250Mi of storage. Set other parameters as per your preference.

3.) Create a deployment named mysql-deployment, use any mysql image as per your preference. Mount the PersistentVolume at mount path /var/lib/mysql.

4.) Create a NodePort type service named mysql and set nodePort to 30007.

5.) Create a secret named mysql-root-pass having a key pair value, where key is password and its value is YUIidhb667, create another secret named mysql-user-pass having some key pair values, where frist key is username and its value is kodekloud_pop, second key is password and value is ksH85UJjhb, create one more secret named mysql-db-url, key name is database and value is kodekloud_db3


6.) Define some Environment variables within the container:

a) name: MYSQL_ROOT_PASSWORD, should pick value from secretKeyRef name: mysql-root-pass and key: password

b) name: MYSQL_DATABASE, should pick value from secretKeyRef name: mysql-db-url and key: database

c) name: MYSQL_USER, should pick value from secretKeyRef name: mysql-user-pass key key: username

d) name: MYSQL_PASSWORD, should pick value from secretKeyRef name: mysql-user-pass and key: password

Note: The kubectl utility on jump_host has been configured to work with the kubernetes cluster.

---

🛠️ Requirements

Create a PersistentVolume named mysql-pv with a capacity of 250Mi.

Create a PersistentVolumeClaim named mysql-pv-claim requesting 250Mi.

Deploy MySQL using a Deployment named mysql-deployment.

Use any MySQL image (e.g., mysql:5.7)

Mount the volume at /var/lib/mysql

Expose MySQL using a Service named mysql of type NodePort on port 30007.

Create Secrets:

mysql-root-pass: key password = YUIidhb667

mysql-user-pass: keys

username = kodekloud_pop

password = ksH85UJjhb

mysql-db-url: key database = kodekloud_db3

Set Environment Variables in the MySQL container:

MYSQL_ROOT_PASSWORD ← from secret mysql-root-pass/password  
MYSQL_DATABASE      ← from secret mysql-db-url/database  
MYSQL_USER          ← from secret mysql-user-pass/username  
MYSQL_PASSWORD      ← from secret mysql-user-pass/password

📄 Full Kubernetes Manifest (mysql-setup.yaml)
```
# -----------------------------
# 1. Persistent Volume
# -----------------------------
apiVersion: v1
kind: PersistentVolume
metadata:
  name: mysql-pv
spec:
  capacity:
    storage: 250Mi
  accessModes:
    - ReadWriteOnce
  hostPath:
    path: /mnt/data/mysql
  persistentVolumeReclaimPolicy: Retain
---
# -----------------------------
# 2. Persistent Volume Claim
# -----------------------------
apiVersion: v1
kind: PersistentVolumeClaim
metadata:
  name: mysql-pv-claim
spec:
  accessModes:
    - ReadWriteOnce
  resources:
    requests:
      storage: 250Mi
---
# -----------------------------
# 3. Secrets
# -----------------------------
apiVersion: v1
kind: Secret
metadata:
  name: mysql-root-pass
type: Opaque
data:
  password: WVVJaWRoYjY2Nw== # base64 of YUIidhb667
---
apiVersion: v1
kind: Secret
metadata:
  name: mysql-user-pass
type: Opaque
data:
  username: a29kZWtsb3VkX3BvcA== # base64 of kodekloud_pop
  password: a3NIOVVKamhi          # base64 of ksH85UJjhb
---
apiVersion: v1
kind: Secret
metadata:
  name: mysql-db-url
type: Opaque
data:
  database: a29kZWtsb3VkX2RiMw== # base64 of kodekloud_db3
---
# -----------------------------
# 4. MySQL Deployment
# -----------------------------
apiVersion: apps/v1
kind: Deployment
metadata:
  name: mysql-deployment
spec:
  replicas: 1
  selector:
    matchLabels:
      app: mysql
  template:
    metadata:
      labels:
        app: mysql
    spec:
      containers:
      - name: mysql
        image: mysql:5.7
        ports:
        - containerPort: 3306
        env:
        - name: MYSQL_ROOT_PASSWORD
          valueFrom:
            secretKeyRef:
              name: mysql-root-pass
              key: password
        - name: MYSQL_DATABASE
          valueFrom:
            secretKeyRef:
              name: mysql-db-url
              key: database
        - name: MYSQL_USER
          valueFrom:
            secretKeyRef:
              name: mysql-user-pass
              key: username
        - name: MYSQL_PASSWORD
          valueFrom:
            secretKeyRef:
              name: mysql-user-pass
              key: password
        volumeMounts:
        - name: mysql-storage
          mountPath: /var/lib/mysql
      volumes:
      - name: mysql-storage
        persistentVolumeClaim:
          claimName: mysql-pv-claim
---
# -----------------------------
# 5. MySQL Service
# -----------------------------
apiVersion: v1
kind: Service
metadata:
  name: mysql
spec:
  selector:
    app: mysql
  type: NodePort
  ports:
    - port: 3306
      targetPort: 3306
      nodePort: 30007
```
## 🚀 Deployment Steps

Apply the manifest:
```bash
kubectl apply -f mysql-setup.yaml
```

## Verify resources:
```
kubectl get pv
kubectl get pvc
kubectl get secrets
kubectl get deploy
kubectl get pods
kubectl get svc
```

Check logs to ensure MySQL started successfully:
```bash
kubectl logs -f deployment/mysql-deployment
```

Confirm NodePort access:
```bash
kubectl get svc mysql
```

MySQL should now be available on:
```
<NodeIP>:30007
```
## 🧠 Key Learnings

- PersistentVolume / PVC: Provide durable storage for databases in Kubernetes.
- Secrets: Securely store sensitive data (like passwords) instead of hardcoding them.
- Deployment: Manages pods declaratively and ensures high availability.
- Service (NodePort): Exposes MySQL to external traffic for testing or external apps.

## 🏁 Final Verification Example
```
kubectl exec -it $(kubectl get pod -l app=mysql -o jsonpath='{.items[0].metadata.name}') -- mysql -u kodekloud_pop -pksH85UJjhb -e "SHOW DATABASES;"
```

Expected output:

+--------------------+
| Database           |
+--------------------+
| information_schema |
| kodekloud_db3      |
+--------------------+

🧰 Cleanup (optional)

To remove all created resources:
```
kubectl delete -f mysql-setup.yaml
```

<img width="1841" height="870" alt="Screenshot 2025-11-09 211006" src="https://github.com/user-attachments/assets/86be9df1-6cab-45b6-bc6b-b663bd4cafc4" />

