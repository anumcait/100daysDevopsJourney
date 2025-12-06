# Day 99 – Attach IAM Policy for DynamoDB Access Using Terraform
### KodeKloud 100 Days of DevOps Challenge – Step-by-Step Guide

---

## Task Overview

Using Terraform, create:

1. A DynamoDB table `xfusion-table`
2. An IAM Role `xfusion-role`
3. An IAM Policy `xfusion-readonly-policy` with read-only DynamoDB permissions
4. Attach the policy to the role
5. Use main.tf, variables.tf, terraform.tfvars, and outputs.tf
6. Ensure `terraform plan` shows **No changes** before submitting

Working directory: `/home/bob/terraform`

---

## Step-by-Step Instructions

---

### 1. Open the Terraform Working Directory

```bash
cd /home/bob/terraform
```

### 2. Create the main.tf File

Create a file named main.tf and add:
```
terraform {
  required_providers {
    aws = {
      source  = "hashicorp/aws"
      version = "~> 5.0"
    }
  }

  required_version = ">= 1.0"
}

provider "aws" {
  region = "us-east-1"
}

resource "aws_dynamodb_table" "xff_table" {
  name           = var.KKE_TABLE_NAME
  billing_mode   = "PAY_PER_REQUEST"
  hash_key       = "id"

  attribute {
    name = "id"
    type = "S"
  }
}

resource "aws_iam_role" "xff_role" {
  name = var.KKE_ROLE_NAME

  assume_role_policy = jsonencode({
    Version = "2012-10-17"
    Statement = [{
      Effect = "Allow"
      Principal = {
        Service = "lambda.amazonaws.com"
      }
      Action = "sts:AssumeRole"
    }]
  })
}

resource "aws_iam_policy" "xff_policy" {
  name        = var.KKE_POLICY_NAME
  description = "Read-only access to specific DynamoDB table"

  policy = jsonencode({
    Version = "2012-10-17"
    Statement = [
      {
        Effect = "Allow"
        Action = [
          "dynamodb:GetItem",
          "dynamodb:Scan",
          "dynamodb:Query"
        ]
        Resource = aws_dynamodb_table.xff_table.arn
      }
    ]
  })
}

resource "aws_iam_role_policy_attachment" "xff_attach" {
  role       = aws_iam_role.xff_role.name
  policy_arn = aws_iam_policy.xff_policy.arn
}
```

---

### 3. Create the variables.tf File
```
variable "KKE_TABLE_NAME" {
  description = "DynamoDB table name"
  type        = string
}

variable "KKE_ROLE_NAME" {
  description = "IAM role name"
  type        = string
}

variable "KKE_POLICY_NAME" {
  description = "IAM policy name"
  type        = string
}
```

---

### 4. Create the terraform.tfvars File
```hcl
KKE_TABLE_NAME  = "xfusion-table"
KKE_ROLE_NAME   = "xfusion-role"
KKE_POLICY_NAME = "xfusion-readonly-policy"
```

---

### 5. Create the outputs.tf File
```
output "kke_dynamodb_table" {
  value = aws_dynamodb_table.xff_table.name
}

output "kke_iam_role_name" {
  value = aws_iam_role.xff_role.name
}

output "kke_iam_policy_name" {
  value = aws_iam_policy.xff_policy.name
}
```

---

### 6. Initialize Terraform
```bash
terraform init
```
---

### 7. Format, Validate, and Apply

```bash
terraform fmt
terraform validate
terraform apply -auto-approve
```
---

### 8. Verify No Changes Are Required

```
terraform plan
```

Expected output:
```
No changes. Your infrastructure matches the configuration.
```

---

### Screenshots

<img width="500" height="300" alt="image" src="https://github.com/user-attachments/assets/d4e697a3-1ee2-41d4-b30c-62e236b80f6e" />

<img width="500" height="300" alt="image" src="https://github.com/user-attachments/assets/2062a75d-3f72-4fb1-8d81-5d702677a8e5" />

<img width="500" height="300" alt="image" src="https://github.com/user-attachments/assets/1381030b-87e7-40de-8df2-1c1d905f603b" />

---






