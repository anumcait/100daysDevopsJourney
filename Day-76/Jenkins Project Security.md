# Day 76: Jenkins Project Security

## Objective
Grant project-level permissions to two Jenkins users (`sam` and `rohan`) for the existing `Packages` job without altering other job configurations.

---

## **Prerequisites**

- Jenkins installed and running.
- Users:
  - `sam` / `sam@pass12345`
  - `rohan` / `rohan@pass12345`
- Job:
  - `Packages`
- Admin credentials:
  - `admin` / `Adm!n321`
- Plugin:
  - **Matrix Authorization Strategy Plugin** installed.

---

## **Step 1: Access Jenkins**

1. Open Jenkins UI via the Jenkins button on the top bar.
2. Login using:
   ```text
   Username: admin
   Password: Adm!n321
    ```

---

## Step 2: Verify Job and Users

1. Confirm the Packages job exists.
2. Confirm users sam and rohan exist.

---

## Step 3: Configure Project-Based Security

| ⚠️ Important: Do not alter global permissions.

1. Go to the Packages job → Click Configure.
2. Scroll down to Enable project-based security.
  - This option appears after installing Matrix Authorization Strategy Plugin.
3. Check Enable project-based security.
4. Select Inherit permissions from parent ACL under inheritance strategy.

---

## Step 4: Add Users and Grant Permissions

For user sam:

1. Click Add user or group → type sam.
2. Grant permissions:

- Read
- Build
- Configure

For user rohan:

1. Click Add user or group → type rohan.
2. Grant permissions:

- Read
- Build
- Cancel
- Configure
- Update
- Tag

| ✅ Make sure only these permissions are assigned. Do not modify other users or jobs.

---

## Step 5: Save and Restart (if needed)

1. Click Save.
2. If plugin installation required a restart:
- Go to Manage Jenkins → Restart Jenkins when installation is complete.
3. Refresh Jenkins UI after restart.

## Step 6: Verification

1. Log in as sam:
  - Ensure he can Read, Build, Configure the Packages job.
  - Cannot perform any other actions.

2. Log in as rohan:

  - Ensure he can Read, Build, Cancel, Configure, Update, Tag the Packages job.
  - Cannot perform actions outside assigned permissions.

---

## Screenshots

<img width="500" height="300" alt="image" src="https://github.com/user-attachments/assets/a76d90a7-e00b-43d4-a7f4-fb651f172045" />

<img width="500" height="300" alt="image" src="https://github.com/user-attachments/assets/ec7d7997-7157-4cb8-b0b1-71aada7a31cf" />

<img width="500" height="300" alt="image" src="https://github.com/user-attachments/assets/9a8dccef-13ce-4ab3-92a2-fda8471cfcf9" />

<img width="500" height="300" alt="image" src="https://github.com/user-attachments/assets/fb95f8b2-0fb7-4b02-a124-9143d88da343" />

<img width="500" height="300" alt="image" src="https://github.com/user-attachments/assets/4d870a94-b947-419c-bcbb-f79e84d7e96a" />






