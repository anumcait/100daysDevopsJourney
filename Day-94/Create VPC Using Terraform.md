# Day 94 – Create VPC Using Terraform  

---

## 📝 Task Summary
The Nautilus DevOps team is migrating their infrastructure to AWS in small, manageable phases.  
Your task is to create an AWS VPC using Terraform.

---

## 🎯 Objective
Create a VPC named **`datacenter-vpc`** in the **`us-east-1`** region with **any IPv4 CIDR block** using Terraform.

Terraform working directory:

```
/home/bob/terraform
```

Only **one** file should be created:

```
main.tf
```

---

## 📁 Directory Structure

```
/home/bob/terraform
 └── main.tf
```

> ⚠️ If a `provider.tf` file exists, DELETE it to avoid duplicate provider configuration errors.

---

## 🛠 Step-by-Step Instructions

### 1️⃣ Navigate to the Terraform working directory
```bash
cd /home/bob/terraform
```

---

### 2️⃣ Create `main.tf` with the following Terraform configuration:

```hcl
provider "aws" {
  region = "us-east-1"
}

resource "aws_vpc" "datacenter_vpc" {
  cidr_block           = "10.0.0.0/16"
  enable_dns_support   = true
  enable_dns_hostnames = true

  tags = {
    Name = "datacenter-vpc"
  }
}
```

---

### 3️⃣ Initialize Terraform
```bash
terraform init
```

---

### 4️⃣ Validate the configuration (optional)
```bash
terraform validate
```

---

### 5️⃣ Apply the Terraform configuration
```bash
terraform apply -auto-approve
```

This will create the VPC in AWS.

---

### 6️⃣ Verify VPC creation

#### Using AWS CLI:
```bash
aws ec2 describe-vpcs --filters "Name=tag:Name,Values=datacenter-vpc"
```

#### Or via AWS Console:
Navigate to: **VPC → Your VPCs → Look for `datacenter-vpc`**

---

### 7️⃣ Cleanup (optional)
If you want to destroy the VPC:

```bash
terraform destroy -auto-approve
```

---

## ✅ Outcome
You have successfully created:

- An AWS VPC  
- Managed using Terraform  
- With DNS support + hostnames enabled  
- Inside **us-east-1**  
- Using a single configuration file (`main.tf`)  

---

## 📌 Notes
- Ensure only **main.tf** exists to avoid provider conflicts.
- CIDR block `10.0.0.0/16` can be replaced with any valid private range.

---

## Screenshots
<img width="500" height="300" alt="image" src="https://github.com/user-attachments/assets/cbc29318-a5d0-4ae0-865d-a958bd46651c" />

<img width="500" height="300" alt="image" src="https://github.com/user-attachments/assets/71fb4981-c97c-4f7b-af5f-67e3fd374d98" />





