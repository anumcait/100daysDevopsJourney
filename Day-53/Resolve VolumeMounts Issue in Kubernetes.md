# Day 53: Resolve VolumeMounts Issue in Kubernetes

## 🧠 Scenario
The `nginx-phpfpm` Pod stopped working due to an issue in its volume mounts.  
It consists of:
- **nginx-container** (serves web pages)
- **php-fpm-container** (runs PHP)

A ConfigMap named `nginx-config` provides the Nginx configuration.

---

## 🔍 Step 1: Verify Pod and ConfigMap

```bash
kubectl get pods
kubectl describe configmap nginx-config
```
Observation:
nginx.conf inside the ConfigMap sets the web root as /var/www/html.
```bash
root /var/www/html;
index index.html index.htm index.php;
```
## ⚠️ Step 2: Identify the Problem

Inspect the current Pod configuration:

kubectl get pod nginx-phpfpm -o yaml > nginx-phpfpm.yaml


Inside the YAML, notice this mismatch:
```bash
- name: nginx-container
  volumeMounts:
    - mountPath: /usr/share/nginx/html
      name: shared-files
```

### 👉 Problem:
Nginx serves files from /usr/share/nginx/html, but PHP-FPM uses /var/www/html.
Because they don’t share the same directory, Nginx cannot find PHP files.

## 🛠️ Step 3: Fix the VolumeMount Path

Open the exported YAML file:

vi nginx-phpfpm.yaml


Find this section:

- image: nginx:latest
  name: nginx-container
  volumeMounts:
    - mountPath: /usr/share/nginx/html
      name: shared-files


Change it to:

    - mountPath: /var/www/html
      name: shared-files


Save and exit (:wq).

## 🧹 Step 4: Delete the Old Pod
```bash
kubectl delete pod nginx-phpfpm
```
## 🚀 Step 5: Recreate the Pod
```bash
kubectl apply -f nginx-phpfpm.yaml
```

Verify the Pod is running:
```bash
kubectl get pods
```

You should see:

nginx-phpfpm   2/2   Running   0   <few seconds>

## 📂 Step 6: Copy the PHP File

Once the Pod is running, copy the PHP file from the jump host into the container:
```bash
kubectl cp /home/thor/index.php nginx-phpfpm:/var/www/html/index.php -c nginx-container
```

Verify inside both containers:
```
kubectl exec -it nginx-phpfpm -c nginx-container -- ls /var/www/html
kubectl exec -it nginx-phpfpm -c php-fpm-container -- ls /var/www/html
```

You should see index.php.

## 🌐 Step 7: Test the Application

Click the Website button on the top bar (or access the exposed service on port 8099).

You should now see the PHP webpage rendered successfully.

## ✅ Summary
|Component|	Path Used|	Fixed|
|---------|----------|-------|
|PHP-FPM|	/var/www/html|	✅
|Nginx|	/usr/share/nginx/html → /var/www/html|	✅

Result:
Both containers now share the same document root (/var/www/html), and the Nginx + PHP-FPM setup works correctly.

<img width="1833" height="838" alt="Screenshot 2025-10-23 215436" src="https://github.com/user-attachments/assets/6208f6ac-6725-4c9a-ae2d-0b401d1cb385" />

