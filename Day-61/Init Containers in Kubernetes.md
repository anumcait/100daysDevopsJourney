# Day 61: Init Containers in Kubernetes

Problem Statement

There are some applications that need to be deployed on Kubernetes cluster and these apps have some pre-requisites where some configurations need to be changed before deploying the app container. Some of these changes cannot be made inside the images so the DevOps team has come up with a solution to use init containers to perform these tasks during deployment. Below is a sample scenario that the team is going to test first.

Create a Deployment named as ic-deploy-xfusion.

Configure spec as replicas should be 1, labels app should be ic-xfusion, template's metadata lables app should be the same ic-xfusion.

The initContainers should be named as ic-msg-xfusion, use image ubuntu with latest tag and use command '/bin/bash', '-c' and 'echo Init Done - Welcome to xFusionCorp Industries > /ic/beta'. The volume mount should be named as ic-volume-xfusion and mount path should be /ic.

Main container should be named as ic-main-xfusion, use image ubuntu with latest tag and use command '/bin/bash', '-c' and 'while true; do cat /ic/beta; sleep 5; done'. The volume mount should be named as ic-volume-xfusion and mount path should be /ic.

Volume to be named as ic-volume-xfusion and it should be an emptyDir type.

Note: The kubectl utility on jump_host has been configured to work with the kubernetes cluster.


---

## 🧠 What Are Init Containers?

In Kubernetes, **Init Containers** are special containers that **run before the main application containers** in a Pod.  
They are designed to perform initialization tasks such as:
- Setting up configuration files.
- Waiting for dependencies to become available.
- Preparing data or directories.

Once all init containers complete successfully, Kubernetes starts the main application container(s).

---

## 🧩 Use Case: xFusionCorp Industries Scenario

The DevOps team at **xFusionCorp Industries** needs to deploy an application that requires a configuration file to exist before the main app container runs.  
Since this setup cannot be pre-baked into the image, they use an **Init Container** to create the required file dynamically before the app starts.

---

## 🎯 Objective

Create a Kubernetes Deployment that:
- Has **1 replica**.
- Uses **1 Init Container** to write a message to a file.
- Has **1 Main Container** that repeatedly prints that file’s content.
- Shares data between the two using an **emptyDir** volume.

---

## ⚙️ Configuration Requirements

| Component | Specification |
|------------|----------------|
| **Deployment Name** | `ic-deploy-xfusion` |
| **App Label** | `ic-xfusion` |
| **Init Container Name** | `ic-msg-xfusion` |
| **Main Container Name** | `ic-main-xfusion` |
| **Image Used** | `ubuntu:latest` |
| **Shared Volume** | `emptyDir`, named `ic-volume-xfusion` |
| **Mount Path** | `/ic` |

---

## 🧱 Step-by-Step Implementation

### Step 1️⃣ – Create the YAML Manifest

Create a file named **`ic-deploy-xfusion.yaml`** and add the following content:

```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: ic-deploy-xfusion
spec:
  replicas: 1
  selector:
    matchLabels:
      app: ic-xfusion
  template:
    metadata:
      labels:
        app: ic-xfusion
    spec:
      # 🧩 Init Container
      initContainers:
      - name: ic-msg-xfusion
        image: ubuntu:latest
        command: ["/bin/bash", "-c", "echo Init Done - Welcome to xFusionCorp Industries > /ic/beta"]
        volumeMounts:
        - name: ic-volume-xfusion
          mountPath: /ic

      # 🚀 Main Container
      containers:
      - name: ic-main-xfusion
        image: ubuntu:latest
        command: ["/bin/bash", "-c", "while true; do cat /ic/beta; sleep 5; done"]
        volumeMounts:
        - name: ic-volume-xfusion
          mountPath: /ic

      # 📦 Shared Volume
      volumes:
      - name: ic-volume-xfusion
        emptyDir: {}
```
---

## Step 2️⃣ – Apply the Deployment

Deploy the manifest using kubectl:

```bash
kubectl apply -f ic-deploy-xfusion.yaml
```
---

## Step 3️⃣ – Verify Deployment and Pods

Check that the deployment and pod are created successfully:
```bash
kubectl get deployments
kubectl get pods
```

Wait until the pod status shows Running.

---

## Step 4️⃣ – Validate Init Container Execution

1. Get the pod name:
```bash
kubectl get pods
```

2. Open a shell inside the running pod:
```bash
kubectl exec -it <pod-name> -- bash
```

2. Check the file created by the init container:
```bash
cat /ic/beta
```

4. Expected Output:
```bash
Init Done - Welcome to xFusionCorp Industries
```

---

## Step 5️⃣ – Check Logs of the Main Container

You can also view the logs of the main container to confirm it’s reading the file repeatedly:
```bash
kubectl logs <pod-name> -c ic-main-xfusion
```
**Expected Output (repeating every 5 seconds):**

Init Done - Welcome to xFusionCorp Industries
Init Done - Welcome to xFusionCorp Industries
Init Done - Welcome to xFusionCorp Industries
...

## Screenshots

<img width="700" height="500" alt="image" src="https://github.com/user-attachments/assets/bc24c9f0-08f6-4f9f-8afd-b6ae44d5c00b" />

<img width="700" height="500" alt="image" src="https://github.com/user-attachments/assets/5255f633-4962-408b-875c-c3f01d9543aa" />

<img width="700" height="500" alt="image" src="https://github.com/user-attachments/assets/668754bf-58c4-41fd-91f3-576378c9527d" />




