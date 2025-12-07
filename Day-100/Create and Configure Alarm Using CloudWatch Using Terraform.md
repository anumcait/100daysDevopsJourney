# Day 100 – Terraform EC2 + CloudWatch Alarm  

## Task Summary

The Nautilus DevOps team must:

1. Launch an EC2 instance named `xfusion-ec2`
2. Create a CloudWatch CPU utilization alarm named `xfusion-alarm`
3. Use existing SNS topic `xfusion-sns-topic`
4. Put all resources in `main.tf`
5. Create `outputs.tf` for:
   - `KKE_instance_name`
   - `KKE_alarm_name`
6. Ensure `terraform plan` returns **No changes**

Terraform working directory:

/home/bob/terraform

---

## Step 1 — Open the Terraform Directory

Open the integrated terminal in VS Code and run:

```bash
cd /home/bob/terraform
```

Verify the folder:
```
ls -l
```

Step 2 — Create main.tf

Create and open:
```
nano main.tf
```
Paste the following:
```
provider "aws" {
  region = "us-east-1"
}

# Existing SNS topic (already created)
resource "aws_sns_topic" "sns_topic" {
  name = "xfusion-sns-topic"
}

# EC2 Instance
resource "aws_instance" "xfusion_ec2" {
  ami           = "ami-0c02fb55956c7d316"
  instance_type = "t2.micro"

  tags = {
    Name = "xfusion-ec2"
  }
}

# CloudWatch Alarm
resource "aws_cloudwatch_metric_alarm" "xfusion_alarm" {
  alarm_name          = "xfusion-alarm"
  comparison_operator = "GreaterThanOrEqualToThreshold"
  evaluation_periods  = 1
  metric_name         = "CPUUtilization"
  namespace           = "AWS/EC2"
  period              = 300
  statistic           = "Average"
  threshold           = 90
  alarm_description   = "Triggers when EC2 CPU exceeds 90% for a 5-minute period"

  alarm_actions = [
    aws_sns_topic.sns_topic.arn
  ]

  dimensions = {
    InstanceId = aws_instance.xfusion_ec2.id
  }
}
```

**Save and exit:**

- CTRL + O, Enter
- CTRL + X

## Step 3 — Create outputs.tf
```
nano outputs.tf
```

---

```
output "KKE_instance_name" {
  value = aws_instance.xfusion_ec2.tags["Name"]
}

output "KKE_alarm_name" {
  value = aws_cloudwatch_metric_alarm.xfusion_alarm.alarm_name
}
```
---

## Step 4 — Initialize Terraform
```
terraform init
```

Expected:
```
Terraform has been successfully initialized!
```

---

Step 5 — Apply the Configuration
terraform apply -auto-approve


Terraform will:

Create EC2 instance

Create CloudWatch CPU alarm

Use existing SNS topic

Step 6 — Verify No Changes Before Submitting
terraform plan


Expected output:

No changes. Your infrastructure matches the configuration.


This confirms the task is complete.

Task Completed

You now have:

- EC2 instance: xfusion-ec2
- CloudWatch alarm: xfusion-alarm
- SNS topic integration
- Outputs for instance and alarm names

---

## Screenshots


<img width="500" height="300" alt="Screenshot 2025-12-07 204728" src="https://github.com/user-attachments/assets/56a265b6-e413-4ae1-adc8-068bc823b854" />

<img width="500" height="300" alt="Screenshot 2025-12-07 210417" src="https://github.com/user-attachments/assets/459bbf80-db22-457f-ac21-5cd69ee71bba" />

<img width="500" height="300" alt="Screenshot 2025-12-07 210530" src="https://github.com/user-attachments/assets/4a8036ed-4c21-4eef-9712-f8739fee30d2" />

---




