# Day 98: Launch EC2 in Private VPC Subnet Using Terraform

The Nautilus DevOps team requires the setup of a **private VPC** with a subnet and a **private EC2 instance**. This ensures resources remain isolated from external networks and communicate securely within the VPC.

---

## **1. Terraform Working Directory**

```bash
cd /home/bob/terraform
```

Open VS Code and use the Integrated Terminal.

---

## **2. Create Variables File (variables.tf)**

```hcl
variable "KKE_VPC_CIDR" {
  description = "CIDR block for the VPC"
  type        = string
  default     = "10.0.0.0/16"
}

variable "KKE_SUBNET_CIDR" {
  description = "CIDR block for the subnet"
  type        = string
  default     = "10.0.1.0/24"
}

```

---

## **3. Create Main Terraform File (main.tf)**

provider "aws" {
  region = "us-east-1" # change as needed
}

# Create private VPC
resource "aws_vpc" "devops_priv_vpc" {
  cidr_block = var.KKE_VPC_CIDR
  tags = {
    Name = "devops-priv-vpc"
  }
}

# Create private Subnet
resource "aws_subnet" "devops_priv_subnet" {
  vpc_id                  = aws_vpc.devops_priv_vpc.id
  cidr_block              = var.KKE_SUBNET_CIDR
  map_public_ip_on_launch = false
  tags = {
    Name = "devops-priv-subnet"
  }
}

# Security Group allowing access only from within VPC
resource "aws_security_group" "devops_priv_sg" {
  name        = "devops-priv-sg"
  description = "Allow access only from within the VPC"
  vpc_id      = aws_vpc.devops_priv_vpc.id

  ingress {
    from_port   = 0
    to_port     = 0
    protocol    = "-1"
    cidr_blocks = [var.KKE_VPC_CIDR]
  }

  egress {
    from_port   = 0
    to_port     = 0
    protocol    = "-1"
    cidr_blocks = ["0.0.0.0/0"]
  }

  tags = {
    Name = "devops-priv-sg"
  }
}

# EC2 instance in private subnet

```hcl
resource "aws_instance" "devops_priv_ec2" {
  ami                    = "ami-0c02fb55956c7d316" # Amazon Linux 2 AMI
  instance_type          = "t2.micro"
  subnet_id              = aws_subnet.devops_priv_subnet.id
  vpc_security_group_ids = [aws_security_group.devops_priv_sg.id]
  associate_public_ip_address = false

  tags = {
    Name = "devops-priv-ec2"
  }

  lifecycle {
    ignore_changes = [
      vpc_security_group_ids
    ]
  }
}

```

---

## **4. Create Outputs File (outputs.tf)**

```hcl
output "KKE_vpc_name" {
  description = "Name of the VPC"
  value       = aws_vpc.devops_priv_vpc.tags["Name"]
}

output "KKE_subnet_name" {
  description = "Name of the Subnet"
  value       = aws_subnet.devops_priv_subnet.tags["Name"]
}

output "KKE_ec2_private" {
  description = "Name of the EC2 instance"
  value       = aws_instance.devops_priv_ec2.tags["Name"]
}

```

---

## **5. Terraform Workflow**

**Step 1: Initialize Terraform**
```bash
terraform init
```

**Step 2: Validate the Plan**
```bash
terraform plan
```

- Ensure the plan matches your expectations.
- If first time applying, it will show resources to be created.

**Step 3: Apply Configuration**
```bash
terraform apply -auto-approve
```
 
- Terraform will create:
  - Private VPC (devops-priv-vpc)
  - Private Subnet (devops-priv-subnet)
  - Security Group allowing only internal VPC traffic
  - EC2 instance (devops-priv-ec2) in the private subnet

**Step 4: Verify**
```bash
terraform plan
```
- After first apply, should display:
```
No changes. Your infrastructure matches the configuration.
```

**Check outputs:**
```
terraform output
```

- Should display:

  - KKE_vpc_name = "devops-priv-vpc"
  - KKE_subnet_name = "devops-priv-subnet"
  - KKE_ec2_private = "devops-priv-ec2"

## Screenshots

<img width="500" height="300" alt="image" src="https://github.com/user-attachments/assets/8d51b154-6e50-42d3-8d02-4e4e7a0e02f7" />

<img width="500" height="300" alt="image" src="https://github.com/user-attachments/assets/ca1afa3b-e382-4bb1-9ee3-9630c4567695" />

<img width="500" height="300" alt="image" src="https://github.com/user-attachments/assets/f7fbe87b-ed70-4e5b-9e89-ef4ab81d2f84" />

<img width="500" height="300" alt="image" src="https://github.com/user-attachments/assets/666ef099-41a0-4f66-826c-f373e8c46afa" />

<img width="500" height="300" alt="image" src="https://github.com/user-attachments/assets/dbdd2ca8-5b4e-4e89-8a43-7b022bbf812d" />

<img width="500" height="300" alt="image" src="https://github.com/user-attachments/assets/1ee6d3e3-c84e-420e-aac0-2485cc07cd8a" />


---


