# Day 96 — Create EC2 Instance Using Terraform  
```
devops challenge The Nautilus DevOps team is strategizing the migration of a portion of their infrastructure to the AWS cloud. Recognizing the scale of this undertaking, they have opted to approach the migration in incremental steps rather than as a single massive transition. To achieve this, they have segmented large tasks into smaller, more manageable units.

For this task, create an EC2 instance using Terraform with the following requirements: The EC2 instance must use the value datacenter-ec2 as its Name tag, which defines the instance name in AWS.

Use the Amazon Linux ami-0c101f26f147fa7fd to launch this instance. The Instance type must be t2.micro.

Create a new RSA key named datacenter-kp. Attach the default (available by default) security group.

The Terraform working directory is /home/bob/terraform. Create the main.tf file (do not create a different .tf file) to provision the instance. Note: Right-click under the EXPLORER section in VS Code and select Open in Integrated Terminal to launch the terminal.

```

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







