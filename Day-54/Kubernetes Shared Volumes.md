# Day 54: Kubernetes Shared Volumes

### Problem Statement:

We are working on an application that will be deployed on multiple containers within a pod on Kubernetes cluster. There is a requirement to share a volume among the containers to save some temporary data. The Nautilus DevOps team is developing a similar template to replicate the scenario. Below you can find more details about it.

Create a pod named volume-share-nautilus.

For the first container, use image debian with latest tag only and remember to mention the tag i.e debian:latest, container should be named as volume-container-nautilus-1, and run a sleep command for it so that it remains in running state. Volume volume-share should be mounted at path /tmp/beta.


For the second container, use image debian with the latest tag only and remember to mention the tag i.e debian:latest, container should be named as volume-container-nautilus-2, and again run a sleep command for it so that it remains in running state. Volume volume-share should be mounted at path /tmp/apps.


Volume name should be volume-share of type emptyDir.


After creating the pod, exec into the first container i.e volume-container-nautilus-1, and just for testing create a file beta.txt with any content under the mounted path of first container i.e /tmp/beta.


The file beta.txt should be present under the mounted path /tmp/apps on the second container volume-container-nautilus-2 as well, since they are using a shared volume.


Note: The kubectl utility on jump_host has been configured to work with the kubernetes cluster.

---

# Kubernetes Shared Volumes: Step-by-Step Guide

## Overview

In this task, we will set up a Kubernetes pod with **two containers** that share a volume. This volume will be of type `emptyDir`, which allows containers within the same pod to share data in real-time. We'll test this by creating a file in the first container's directory and verifying that it appears in the second container's directory.

### **Task Requirements:**
- Create a pod named `volume-share-nautilus`.
- The pod should contain two containers, both using the `debian:latest` image.
- **Container 1** should mount a shared volume at `/tmp/beta`.
- **Container 2** should mount the same shared volume at `/tmp/apps`.
- **Volume Type**: `emptyDir` (temporary, shared storage).
- Create a file in the first container’s `/tmp/beta` directory and verify it’s accessible in the second container’s `/tmp/apps` directory.

---

## Step 1: Create the Pod Definition YAML

Start by creating a YAML file for the pod that includes the shared volume configuration.

```yaml
apiVersion: v1
kind: Pod
metadata:
  name: volume-share-nautilus
spec:
  containers:
  - name: volume-container-nautilus-1
    image: debian:latest
    command: ["sleep", "infinity"]  # Keep the container running
    volumeMounts:
    - mountPath: /tmp/beta
      name: volume-share  # Mount the volume at /tmp/beta in the first container
  - name: volume-container-nautilus-2
    image: debian:latest
    command: ["sleep", "infinity"]  # Keep the container running
    volumeMounts:
    - mountPath: /tmp/apps
      name: volume-share  # Mount the volume at /tmp/apps in the second container
  volumes:
  - name: volume-share
    emptyDir: {}  # Define the shared emptyDir volume
```
**Explanation:**

- volume-container-nautilus-1 mounts the shared volume at /tmp/beta.
- volume-container-nautilus-2 mounts the shared volume at /tmp/apps.
- The emptyDir volume is shared between the two containers.

## Step 2: Apply the Pod Definition

Apply the YAML file to your Kubernetes cluster to create the pod.
```bash
kubectl apply -f volume-share-nautilus-pod.yaml
```

This will create the pod volume-share-nautilus with two containers running in it.

## Step 3: Verify the Pod Status

Ensure that the pod is up and running:
```bash
kubectl get pods
```
You should see volume-share-nautilus in the list of pods with a status of Running.

## Step 4: Exec into the First Container and Create the File

Now, exec into the first container to create a file under the mounted path /tmp/beta:
```bash
kubectl exec -it volume-share-nautilus -c volume-container-nautilus-1 -- bash
```
Inside the container, create a file named beta.txt:
```bash
echo "This is a test file for shared volume" > /tmp/beta/beta.txt
```

This file will be stored in the shared emptyDir volume.

## Step 5: Verify the File in the Second Container

Next, exec into the second container to check if the file is present in its mounted directory /tmp/apps:

Inside the container, create a file named beta.txt:
```bash
kubectl exec -it volume-share-nautilus -c volume-container-nautilus-2 -- bash
```
List the contents of /tmp/apps:

```bash
ls /tmp/apps
```
You should see beta.txt listed here, indicating that the file was successfully shared between the two containers using the shared volume.

Step 6: Clean Up

Once you’ve verified the functionality, you can delete the pod to clean up the resources:
```bash
kubectl delete pod volume-share-nautilus
```

## Key Takeaways

- Shared Volumes: The emptyDir volume allows containers in the same pod to share data seamlessly.
- Persistence: The data in an emptyDir volume exists as long as the pod is running. Once the pod is deleted, the data is lost.
- Use Cases: This pattern is helpful for use cases like logging, temporary storage, and inter-container communication in microservices architectures.

## Screenshots

<img width="500" height="300" alt="Screenshot 2025-10-24 201719" src="https://github.com/user-attachments/assets/71185077-c707-4a8d-8bfb-73a8aa3f129d" />
<img width="500" height="300" alt="Screenshot 2025-10-24 201737" src="https://github.com/user-attachments/assets/33ea2512-22f4-4420-b400-943f1f0112be" />





