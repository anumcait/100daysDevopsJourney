# Day 50: Set Resource Limits in Kubernetes Pods

The Nautilus DevOps team has noticed performance issues in some Kubernetes-hosted applications due to resource constraints. To address this, they plan to set limits on resource utilization. Here are the details:


Create a pod named httpd-pod with a container named httpd-container. Use the httpd image with the latest tag (specify as httpd:latest). Set the following resource limits:

Requests: Memory: 15Mi, CPU: 100m

Limits: Memory: 20Mi, CPU: 100m

Note: The kubectl utility on jump_host is configured to operate with the Kubernetes cluster.

## Objective:
The goal of this exercise is to create a Kubernetes pod named `httpd-pod` with a container named `httpd-container` that uses the `httpd:latest` image. The pod should have resource requests and limits for both memory and CPU set to specific values to manage performance effectively.

### Resource Requirements:
- **Requests**:
  - Memory: 15Mi
  - CPU: 100m
- **Limits**:
  - Memory: 20Mi
  - CPU: 100m

## Steps:

### 1. Create a YAML Configuration File for the Pod

To specify the pod configuration, create a YAML file named `httpd-pod.yaml` (or another name of your choice) with the following content:

```yaml
apiVersion: v1
kind: Pod
metadata:
  name: httpd-pod
spec:
  containers:
  - name: httpd-container
    image: httpd:latest
    resources:
      requests:
        memory: "15Mi"
        cpu: "100m"
      limits:
        memory: "20Mi"
        cpu: "100m"
```
**Explanation:**
- apiVersion: v1: The API version used to define the pod.
- kind: Pod: Specifies the type of Kubernetes object being created (a pod in this case).
- metadata: Contains metadata about the pod, such as its name (httpd-pod).
- spec: Describes the pod specification.
  - containers: Defines the containers within the pod.
    - name: The name of the container inside the pod (httpd-container).
    - image: The image used for the container (httpd:latest).
    - resources: Specifies the resource requests and limits:
    - requests: The minimum amount of resources the container needs.
    - limits: The maximum resources the container is allowed to use.
## 2. Apply the YAML File to Create the Pod
Once the YAML file is created, use the following kubectl command to apply the configuration and create the pod in the Kubernetes cluster:

```
kubectl apply -f httpd-pod.yaml
```
## 3. Verify the Pod Configuration
To ensure that the pod has been created correctly with the desired resource requests and limits, run the following command:

```
kubectl get pod httpd-pod -o yaml
```
This will display the pod's YAML configuration, where you can verify that the resource limits and requests have been applied as expected.

<img width="700" height="500" alt="Screenshot 2025-10-19 134234" src="https://github.com/user-attachments/assets/9be8962a-61e8-4c5b-8965-8aee57603c87" />


