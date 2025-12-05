# Day 95 – Create AWS Security Group Using Terraform  
## Step-by-Step Guide (RAW Markdown)

---

## Task Overview

The Nautilus DevOps team is migrating infrastructure to AWS in phases.  
Your task for this step is to create an AWS Security Group using Terraform with these exact requirements:

- Security Group Name: **devops-sg**
- Description: **Security group for Nautilus App Servers**
- Inbound Rule 1: HTTP → port **80** from **0.0.0.0/0**
- Inbound Rule 2: SSH → port **22** from **0.0.0.0/0**
- Region: **us-east-1**
- Use the **default VPC**
- Work inside directory: **/home/bob/terraform**
- Create only one file: **main.tf**

---

# Step-by-Step Instructions

---

## 1. Open the Terraform Working Directory

Open VS Code, then run:
```
cd /home/bob/terraform
```

Or use:

- Right-click under the **Explorer** in VS Code  
- Select **Open in Integrated Terminal**

---

## 2. Create the main.tf File

Inside `/home/bob/terraform`, create:

```
main.tf
```

---

## 3. Paste the Following Terraform Configuration

```hcl
terraform {
  required_providers {
    aws = {
      source  = "hashicorp/aws"
      version = "~> 5.0"
    }
  }
}

provider "aws" {
  region = "us-east-1"
}

# Get the default VPC
data "aws_vpc" "default" {
  default = true
}

# Create the security group
resource "aws_security_group" "devops_sg" {
  name        = "devops-sg"
  description = "Security group for Nautilus App Servers"
  vpc_id      = data.aws_vpc.default.id

  # HTTP inbound rule
  ingress {
    description = "Allow HTTP"
    from_port   = 80
    to_port     = 80
    protocol    = "tcp"
    cidr_blocks = ["0.0.0.0/0"]
  }

  # SSH inbound rule
  ingress {
    description = "Allow SSH"
    from_port   = 22
    to_port     = 22
    protocol    = "tcp"
    cidr_blocks = ["0.0.0.0/0"]
  }

  # Allow all outbound traffic
  egress {
    from_port   = 0
    to_port     = 0
    protocol    = "-1"
    cidr_blocks = ["0.0.0.0/0"]
  }

  tags = {
    Name = "devops-sg"
  }
}
```
## 4. Initialize Terraform

Run:
```csharp
terraform init
```
## 5. Validate the Configuration
```
terraform validate
```

Expected output:
```
Success! The configuration is valid.

```

---

### 6. Review Planned Changes
```
terraform plan
```

Terraform will show that it plans to create a security group.

### 7. Apply the Configuration
```
terraform apply -auto-approve
```
### Screenshots

<img width="500" height="300" alt="image" src="https://github.com/user-attachments/assets/e068ee51-0ab4-4f0d-ad50-ce5915e7a2e0" />

<img width="500" height="300" alt="image" src="https://github.com/user-attachments/assets/34ab6e2e-f228-4a50-997d-79c1f47a0cb9" />

<img width="500" height="300" alt="image" src="https://github.com/user-attachments/assets/feceaa8d-fb08-41be-8ab7-9a63c6e077ab" />

<img width="500" height="300" alt="image" src="https://github.com/user-attachments/assets/63c1d5ec-dac8-434a-b77e-692665531905" />

<img width="500" height="300" alt="image" src="https://github.com/user-attachments/assets/9ef96d27-76b3-4bfc-a807-0abed53408b1" />







