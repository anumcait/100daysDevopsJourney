# Day 57: Print Environment Variables

## Problem Statement
```
The Nautilus DevOps team is working on to setup some pre-requisites for an application that will send the greetings to different users. There is a sample deployment, that needs to be tested. Below is a scenario which needs to be configured on Kubernetes cluster. Please find below more details about it.


Create a pod named print-envars-greeting.

Configure spec as, the container name should be print-env-container and use bash image.

Create three environment variables:

a. GREETING and its value should be Welcome to

b. COMPANY and its value should be DevOps

c. GROUP and its value should be Group

Use command ["/bin/sh", "-c", 'echo "$(GREETING) $(COMPANY) $(GROUP)"'] (please use this exact command), also set its restartPolicy policy to Never to avoid crash loop back.

You can check the output using kubectl logs -f print-envars-greeting command.


Note: The kubectl utility on jump_host has been configured to work with the kubernetes cluster.
```

## Task Requirements

- **Pod Name:** `print-envars-greeting`
- **Container Name:** `print-env-container`
- **Image:** `bash`
- **Environment Variables:**
  - `GREETING` = `Welcome to`
  - `COMPANY` = `DevOps`
  - `GROUP` = `Group`
- **Command:**  
  ```bash
  ["/bin/sh", "-c", 'echo "$(GREETING) $(COMPANY) $(GROUP)"']
```
- Restart Policy: **Never**
---

## Kubernetes YAML

```bash
apiVersion: v1
kind: Pod
metadata:
  name: print-envars-greeting
spec:
  restartPolicy: Never
  containers:
  - name: print-env-container
    image: bash
    env:
    - name: GREETING
      value: "Welcome to"
    - name: COMPANY
      value: "DevOps"
    - name: GROUP
      value: "Group"
    command: ["/bin/sh", "-c", 'echo "$(GREETING) $(COMPANY) $(GROUP)"']
```
---

## Steps to Deploy

**1.** Save the YAML file as print-envars-greeting.yaml.
**2.** Apply the YAML to create the pod:
```bash
kubectl apply -f print-envars-greeting.yaml
```
**3.** Check pod status:
```bash
kubectl get pods
```
**4.** View pod output:
```bash
kubectl logs -f print-envars-greeting
```
**5.** Expected Output:
```
Welcome to DevOps Group
```

## Notes:
- Using restartPolicy: Never prevents crash loops since the pod completes after printing the message.
- This is a simple demonstration of setting environment variables in Kubernetes pods.

## Screenshot

<img width="700" height="500" alt="image" src="https://github.com/user-attachments/assets/cea09bfe-9739-49d5-97ff-46cf8ad2f751" />



  
