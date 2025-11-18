# Day 72: Jenkins Parameterized Builds

## 🎯 Task Objective

A new DevOps engineer has joined the team, and we need to test a simple Jenkins parameterized job to verify understanding of parameterized builds.

This task involves creating a parameterized Jenkins job with a string and choice parameter, configuring a shell build step, and triggering the job.

---

## 🔐 Login to Jenkins

Use the Jenkins button in the environment page to access the UI.

Credentials:
```
Username: admin
Password: Adm!n321
```
---

## 🛠️ Step-by-Step Instructions
### 1️⃣ Create a Freestyle Job: parameterized-job

1. From the Jenkins dashboard, click New Item.
2. Enter the item name: parameterized-job.
3. Select Freestyle project.
4. Click OK.

### 2️⃣ Add a String Parameter

1. In the job configuration page, enable This project is parameterized.
2. Click Add Parameter → String Parameter.
3. Fill in:
```
Name: Stage
Default Value: Build
```
---

### 3️⃣ Add a Choice Parameter

1. Click Add Parameter → Choice Parameter.
2. Fill in:
```
Name: env
Choices:
Development
Staging
Production
```

### 4️⃣ Add Shell Build Step
1. Scroll to the Build section.
2. Click Add build step → Execute shell.
3. Add the following script:
```
echo "Stage value: $Stage"
echo "Environment selected: $env"
```

---

### 5️⃣ Build the Job

1. Navigate to the job's home page.
2. Click Build with Parameters.
3. Select:
  - env: Staging
  - Stage: (leave default Build)
4. Click Build.
5. Open the latest build → Console Output to verify:
```
Stage value: Build
Environment selected: Staging
```

---

## Screenshots

<img width="500" height="300" alt="image" src="https://github.com/user-attachments/assets/6c909e67-c2fb-412e-bb2d-bfef4f2b7b24" />
<img width="500" height="300" alt="image" src="https://github.com/user-attachments/assets/17aab336-c050-45b3-a710-53bd8f05e5e7" />
<img width="500" height="300" alt="image" src="https://github.com/user-attachments/assets/59b94e71-9b3c-4b6b-b94f-aa7a7e4f26ec" />
<img width="500" height="300" alt="image" src="https://github.com/user-attachments/assets/48e08a82-9c30-4d6a-b2cc-634097571774" />
<img width="500" height="300" alt="image" src="https://github.com/user-attachments/assets/175e286c-649e-4091-a475-b2bb4534e4f0" />
<img width="500" height="300" alt="image" src="https://github.com/user-attachments/assets/086fe7fe-23e2-4026-8d69-906adeec22a9" />
<img width="500" height="300" alt="image" src="https://github.com/user-attachments/assets/f755102f-134e-43c2-b577-fac149a2a1f4" />
<img width="500" height="300" alt="image" src="https://github.com/user-attachments/assets/52a597e3-2a9b-46e7-9a22-d1477253ecca" />










