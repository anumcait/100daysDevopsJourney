# 📘 Day 70: Configure Jenkins User Access (User: Javed)


This document explains how to configure Jenkins user access using **Project-based Matrix Authorization Strategy**, create user **Javed**, and give him **read-only access** to the existing job **Helloworld**.

---

## ✅ **1. Login to Jenkins**
1. Open your Jenkins URL.
2. Login using credentials:
   - **Username:** `admin`  
   - **Password:** `Adm!n321`
3. Click the *Jenkins* button on the top bar if needed to reach the main dashboard.

---

## ✅ **2. Install Required Plugins**
1. Go to **Manage Jenkins** → **Manage Plugins**.
2. Under **Available**, search for:  
   **Project-based Matrix Authorization Strategy**
3. Select the plugin → **Install without restart**.
4. After installation completes, click:  
   **Restart Jenkins when installation is complete**
5. Wait until the login screen returns (do NOT click Finish early).

---

## ✅ **3. Create New User: Javed**
1. Go to **Manage Jenkins** → **Manage Users**.
2. Click **Create User**.
3. Fill in the details:
   - **Username:** `Javed`
   - **Password:** `LQfKeWWxWD`
   - **Full Name:** `Javed`
   - **Email:** *(optional)*
4. Click **Create User**.

---

## ✅ **4. Enable Project-based Matrix Authorization**
1. Go to **Manage Jenkins** → **Configure Global Security**.
2. Under **Authorization**, select:  
   **Project-based Matrix Authorization Strategy**
3. A permissions matrix will appear.

---

## ✅ **5. Set Global Permissions**
### **For user: admin**
- Keep **Overall → Administer** checked.  
(Do NOT remove; otherwise you will lock yourself out.)

### **For user: Javed**
- Add user `Javed` if not already listed.
- Grant only:
  - ✔ **Overall → Read**

### **For user: Anonymous**
- Remove *all* permissions  
  (no boxes should be checked).

Click **Save**.

---

## ✅ **6. Configure Job-Level Permissions for Helloworld**
This is the most important step.  
Validation fails if you skip this.

1. Go to Jenkins Dashboard.
2. Click job **Helloworld**.
3. Click **Configure**.
4. Scroll to the **bottom** to find:  
   **Project-based Matrix Authorization Strategy**

(If you don’t see it, the plugin is missing or global strategy wasn't set.)

### **Add user: Javed**
- Click **Add user or group**
- Type: `Javed`
- Click **OK**

### **Give ONLY the following permission:**

| Permission Category | Permission | Javed |
|--------------------|------------|--------|
| Job                | Read       | ✔️      |

### **Make sure these are NOT checked for Javed:**
- Configure ❌  
- Build ❌  
- Discover ❌  
- Workspace ❌  
- Delete ❌  
- Cancel ❌  
- Anything else ❌  

### **Anonymous**
- No permissions.

### **Admin user**
- Keep full permissions.

Click **Save**.

---

## 🎯 **EXPECTED RESULT**
- Javed **can view the Helloworld job**  
- Javed **cannot edit, build, delete, or configure anything**  
- Admin has full control  
- Anonymous has zero access

---

## 📸 **7. Capture Screenshots for Submission**
Take screenshots of:
1. User creation page (Javed)
2. Global Security permissions matrix
3. Job-level permission matrix for Helloworld
4. Successful login as Javed (if needed)

You may also record with Loom (optional).

---

# ✅ **DONE! Your Jenkins access is configured correctly.**

<img width="1881" height="846" alt="Screenshot 2025-11-12 220036" src="https://github.com/user-attachments/assets/19a4d62d-5a7f-40fb-a01d-11fbadcf1e1f" />

<img width="1884" height="822" alt="Screenshot 2025-11-14 205806" src="https://github.com/user-attachments/assets/880f0a54-92aa-4412-850f-9b24c0e4d499" />

<img width="1885" height="869" alt="Screenshot 2025-11-14 210540" src="https://github.com/user-attachments/assets/4d5f0e62-cf2c-4520-9f20-940ec671923d" />

<img width="1886" height="893" alt="Screenshot 2025-11-14 212439" src="https://github.com/user-attachments/assets/546e6e96-b540-4273-be71-f84f2cc1aad1" />

<img width="1871" height="901" alt="Screenshot 2025-11-14 212728" src="https://github.com/user-attachments/assets/ccd55eb6-4cae-4744-8238-d92ba6778766" />

