```markdown
# AWS EC2 Monitoring and Alerting with CloudWatch & SNS

This project demonstrates how to deploy a monitored EC2 instance using **Terraform**, with integrated **CloudWatch alarms** and **SNS email notifications**. It simulates a real-world CloudOps scenario of handling high CPU usage alerts using a stress test tool.

---

## Skills Demonstrated

- Infrastructure as Code (Terraform)
- AWS EC2 provisioning
- CloudWatch Alarms
- SNS Topics & Email Subscriptions
- IAM Roles & Policies
- Event-driven monitoring automation

---

## Setup Instructions

### Step 1: Clone the Repo

```bash
git clone https://github.com/your-username/aws-ec2-monitoring-alerts.git
cd aws-ec2-monitoring-alerts
````

---

### Step 2: Configure `terraform.tfvars`

Update this file with your specific values:

```hcl
region       = "us-east-1"
ami_id       = "ami-0c2b8ca1dad447f8a" # Amazon Linux 2 AMI
key_pair     = "your-key-name"
alert_email  = "your.email@example.com"
```

> Make sure the AMI ID matches the region you select, and that your key pair exists in the target AWS region.

---

### Step 3: Deploy with Terraform

```bash
terraform init
terraform apply
```

* Confirm the SNS subscription via the email you receive.
* An EC2 instance will launch with a CloudWatch alarm and monitoring policy.

---

## Stress Test & Trigger Alarm

The EC2 instance uses a **User Data script** (`stress-cpu.sh`) that:

* Installs the `stress` package
* Triggers high CPU load for 5 minutes

The CloudWatch alarm triggers when:

* **CPU Utilization > 80%**
* For **2 consecutive minutes**

When the alarm state goes to `ALARM`, an email is sent to your configured address via SNS.

---

## IAM Role

The EC2 instance is assigned an IAM role with the `CloudWatchAgentServerPolicy` managed policy attached, which allows it to push metrics to CloudWatch.

---

## Outputs

The Terraform configuration can output useful info like:

* EC2 Public IP
* SNS Topic ARN
* CloudWatch Alarm Name

Example from `outputs.tf`:

```hcl
output "ec2_public_ip" {
  value = aws_instance.monitored_instance.public_ip
}

output "cloudwatch_alarm_name" {
  value = aws_cloudwatch_metric_alarm.cpu_alarm.alarm_name
}
```

---

## Teardown

To destroy all resources when you're done:

```bash
terraform destroy
```

---

## Troubleshooting Notes

| Issue                | Resolution                                                               |
| -------------------- | ------------------------------------------------------------------------ |
| Alarm didn’t trigger | Wait a few minutes or manually SSH and run stress tool                   |
| No email received    | Confirm SNS email subscription (check inbox/spam)                        |
| Terraform error      | Ensure AWS CLI is configured (`aws configure`) and credentials are valid |
| SSH timeout          | Check that security group allows inbound access on port 22               |

---

## Manual Test Commands

If you want to SSH in and trigger CPU load manually:

```bash
# SSH into the instance
ssh -i ~/your-key.pem ec2-user@<instance_public_ip>

# Install stress if not already installed
sudo yum install -y stress

# Run stress test
stress --cpu 2 --timeout 300
```

---

## License

MIT

---

## Tags

`#terraform` `#cloudwatch` `#sns` `#monitoring` `#alerting` `#aws` `#iac` `#devops` `#cloudops`

---

## ✅ Project Status

✔️ EC2 Deployed
✔️ CloudWatch Alarm Active
✔️ SNS Notification Confirmed
✔️ Terraform Automated
