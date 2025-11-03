# Day 62: Manage Secrets in Kubernetes

Problem Statement:
The Nautilus DevOps team is working to deploy some tools in Kubernetes cluster. Some of the tools are licence based so that licence information needs to be stored securely within Kubernetes cluster. Therefore, the team wants to utilize Kubernetes secrets to store those secrets. Below you can find more details about the requirements:

We already have a secret key file blog.txt under /opt location on jump host. Create a generic secret named blog, it should contain the password/license-number present in blog.txt file.

Also create a pod named secret-devops.

Configure pod's spec as container name should be secret-container-devops, image should be debian with latest tag (remember to mention the tag with image). Use sleep command for container so that it remains in running state. Consume the created secret and mount it under /opt/demo within the container.

To verify you can exec into the container secret-container-devops, to check the secret key under the mounted path /opt/demo. Before hitting the Check button please make sure pod/pods are in running state, also validation can take some time to complete so keep patience.

Note: The kubectl utility on jump_host has been configured to work with the kubernetes cluster.

---

# 🗝️ Day 62: Manage Secrets in Kubernetes — KodeKloud 100 Days of DevOps Challenge

## 🎯 Task Summary

The Nautilus DevOps team wants to securely store license information within a Kubernetes cluster using **Kubernetes Secrets**.

### Requirements:
1. A file `/opt/blog.txt` already exists on the jump host.
2. Create a **generic secret** named `blog` using this file.
3. Create a **pod** named `secret-devops`:
   - Container name: `secret-container-devops`
   - Image: `debian:latest`
   - Command: `sleep 3600`
   - Mount the secret under `/opt/demo` inside the container.
4. Verify that the pod can access the secret.

---

## 🧩 Step 1: Verify the Source File

Check that the secret file exists and view its contents.

```bash
ls -l /opt/blog.txt
cat /opt/blog.txt
```
## 🔐 Step 2: Create the Secret

Create a generic secret named blog from the file /opt/blog.txt.
```bash
kubectl create secret generic blog --from-file=/opt/blog.txt
```

Verify the secret:
```bash
kubectl get secrets
kubectl describe secret blog
```

(Optional: To view encoded data)

```
kubectl get secret blog -o yaml
```
---

## 🧱 Step 3: Create the Pod Manifest

Create a pod definition file named secret-pod.yaml.
```
cat <<EOF > secret-pod.yaml
apiVersion: v1
kind: Pod
metadata:
  name: secret-devops
spec:
  containers:
  - name: secret-container-devops
    image: debian:latest
    command: ["sleep", "3600"]
    volumeMounts:
    - name: blog-secret
      mountPath: /opt/demo
  volumes:
  - name: blog-secret
    secret:
      secretName: blog
EOF
```

Apply the manifest:
```
kubectl apply -f secret-pod.yaml
```
---

## 🔍 Step 4: Verify the Pod Status

Check that the pod is created and running:
```
kubectl get pods
```

Wait until the secret-devops pod shows STATUS: Running.

---

## 🧪 Step 5: Verify Secret Mount Inside Pod

Exec into the running container:
```
kubectl exec -it secret-devops -- /bin/bash
```

Inside the container, verify the secret file:
```
ls /opt/demo
cat /opt/demo/blog.txt
```

You should see the content from /opt/blog.txt.

Exit from the pod:
```
exit
```
---

## 🧹 Step 6: Clean Up (Optional)

If you wish to delete the created resources:
```bash
kubectl delete pod secret-devops
kubectl delete secret blog
```
---

## 📘 Summary

In this task, you learned to:

- Create Kubernetes Secrets from files.
- Mount secrets as volumes into containers.
- Verify and manage secret-based configurations securely.
