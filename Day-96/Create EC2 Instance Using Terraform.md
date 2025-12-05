# Day 96 — Create EC2 Instance Using Terraform  
## KodeKloud 100 Days of DevOps Challenge  
### Full Step-by-Step Guide (RAW Markdown)

---

## 📁 1. Navigate to the Terraform working directory

```bash
cd /home/bob/terraform
```
✍️ 2. Create the main.tf file
```
nano main.tf
```
Paste the complete Terraform configuration below.

📄 main.tf

```
provider "aws" {
  region = "us-east-1"
}

# Generate RSA key pair
resource "tls_private_key" "datacenter" {
  algorithm = "RSA"
  rsa_bits  = 4096
}

# Create AWS key pair using generated public key
resource "aws_key_pair" "datacenter_kp" {
  key_name   = "datacenter-kp"
  public_key = tls_private_key.datacenter.public_key_openssh
}

# Get default VPC
data "aws_vpc" "default" {
  default = true
}

# Get default Security Group
data "aws_security_group" "default" {
  name   = "default"
  vpc_id = data.aws_vpc.default.id
}

# EC2 Instance
resource "aws_instance" "datacenter_ec2" {
  ami           = "ami-0c101f26f147fa7fd"
  instance_type = "t2.micro"
  key_name      = aws_key_pair.datacenter_kp.key_name

  vpc_security_group_ids = [
    data.aws_security_group.default.id
  ]

  tags = {
    Name = "datacenter-ec2"
  }
}


```
---

## 📦 3. Initialize Terraform
```
terraform init
```
---

## 🔍 4. Validate the configuration
```
terraform validate
```
---

## 📘 5. Preview the execution plan
terraform plan


Expected resources:

- 1 TLS private key
- 1 AWS key pair: datacenter-kp
- 1 EC2 instance: datacenter-ec2

🚀 6. Apply the configuration
terraform apply -auto-approve


## Terraform will:

- Generate a 4096-bit RSA key
- Upload its public key as datacenter-kp
- Launch an EC2 instance using:
    AMI: ami-0c101f26f147fa7fd
    Instance type: t2.micro
    Default security group
    Name tag: datacenter-ec2

## 🧹 7. (Optional) Destroy all resources
```
terraform destroy -auto-approve
```
## Screenshots

<img width="500" height="300" alt="image" src="https://github.com/user-attachments/assets/2406afc7-8d4c-47b7-b8a6-494fb9e69116" />

<img width="500" height="300" alt="image" src="https://github.com/user-attachments/assets/91cc4ac2-ec79-4af1-bfb9-e8c3a0f12278" />

<img width="500" height="300" alt="image" src="https://github.com/user-attachments/assets/a8c4435a-4404-40a8-a063-513788e9fb9d" />

<img width="500" height="300" alt="image" src="https://github.com/user-attachments/assets/73b4e284-8953-419c-b874-7a0a18626e20" />

<img width="500" height="300" alt="image" src="https://github.com/user-attachments/assets/a2d496bf-56a4-4915-a9c2-0c981a998d6d" />







