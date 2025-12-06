# Day 97 — Create IAM Policy Using Terraform

When building infrastructure on AWS, Identity and Access Management (IAM) is one of the first and most critical components to configure. IAM enables the creation and management of users, groups, roles, and policies that define access to AWS services.

For this task, the Nautilus DevOps team needs to create an IAM Policy using Terraform that allows read-only access to the EC2 console.

---

## Task Requirements

- Create an IAM policy named `iampolicy_ammar`
- Region must be `us-east-1`
- Policy must allow read-only access to EC2:
  - View EC2 instances
  - View AMIs
  - View snapshots
- Use Terraform working directory: `/home/bob/terraform`
- Create only `main.tf`

---

## main.tf — Terraform Code

```hcl
provider "aws" {
  region = "us-east-1"
}

resource "aws_iam_policy" "iampolicy_ammar" {
  name        = "iampolicy_ammar"
  description = "IAM policy for read-only access to EC2"

  policy = jsonencode({
    Version = "2012-10-17"
    Statement = [
      {
        Effect   = "Allow"
        Action   = [
          "ec2:DescribeInstances",
          "ec2:DescribeImages",
          "ec2:DescribeSnapshots",
          "ec2:DescribeVolumes",
          "ec2:DescribeTags",
          "ec2:DescribeKeyPairs",
          "ec2:DescribeSecurityGroups",
          "ec2:DescribeNetworkInterfaces",
          "ec2:DescribeAddresses",
          "ec2:DescribeAvailabilityZones",
          "ec2:DescribeRegions",
          "ec2:DescribeVpcs",
          "ec2:DescribeSubnets"
        ]
        Resource = "*"
      }
    ]
  })
}

```
---

## Deploy Instructions
```
cd /home/bob/terraform
terraform init
terraform plan
terraform apply
```
Type `yes` when prompted.

---

**Result**

Terraform creates an IAM policy named iampolicy_ammar in us-east-1 with EC2 read-only access.


## Screenshots

<img width="500" height="300" alt="image" src="https://github.com/user-attachments/assets/ed34fa3d-5fca-403b-9da3-65eb3951230c" />
<img width="500" height="300" alt="image" src="https://github.com/user-attachments/assets/39b1c243-47de-4c6b-a825-254604853cfc" />
<img width="500" height="300" alt="image" src="https://github.com/user-attachments/assets/c99d00cc-6cce-4efb-bf2f-c883ca29c565" />
<img width="500" height="300" alt="image" src="https://github.com/user-attachments/assets/f39ff9c1-a470-4dd0-8682-71d7b442a98f" />








