
# AWS Infrastructure Setup using Terraform

This project automates the creation of infrastructure on AWS using Terraform. It sets up a scalable environment with EC2 instances behind an Application Load Balancer (ALB), public subnets, and integration with Amazon S3.

---

## 🚀 Architecture Overview

[AWS Infra Diagram] ![AWS Terraform Infra](https://github.com/user-attachments/assets/772d6eb4-d72b-4445-b817-f6b1e23d44fb)

- **VPC**: Custom Virtual Private Cloud with public subnets.
- **EC2 Instances**: Multiple EC2 instances running Windows or Linux, configured via `userdata.sh`.
- **Application Load Balancer (ALB)**: Distributes incoming traffic to EC2 instances.
- **Amazon S3**: Connected for storage of static files or application data.

---

## 📁 Project Structure

```
terraform/
│
├── main.tf                  # Main infrastructure resources definition
├── provider.tf              # AWS provider configuration
├── variables.tf             # Variables used in the project
├── userdata.sh              # Shell script for EC2 instance initialization
├── userdata1.sh             # (Optional) Additional user data script
├── terraform.tfstate        # Terraform state file (auto-generated)
├── terraform.tfstate.backup # Terraform state backup
├── terraform.lock.hcl       # Lock file to ensure consistent provider versions
├── terraform-provider-aws_v5.10.0_x5.exe  # AWS provider binary
```

---

## ⚡ Prerequisites

1. Install [Terraform].
2. AWS Account with access credentials (Access Key & Secret Key).
3. Set up AWS CLI or export credentials using environment variables:
    ```bash
    export AWS_ACCESS_KEY_ID="your-access-key-id"
    export AWS_SECRET_ACCESS_KEY="your-secret-access-key"
    ```

---

## ✅ Usage Instructions

1. Initialize the Terraform project:
    ```bash
    terraform init
    ```

2. Review the planned infrastructure:
    ```bash
    terraform plan
    ```

3. Apply the configuration to create resources:
    ```bash
    terraform apply
    ```
    - Confirm with `yes` when prompted.

4. Outputs:
    After apply completes, you will see output like:
    ```bash
    loadbalancerdns = "myalb-xxxxxxx.us-east-1.elb.amazonaws.com"
    ```

---

## 📜 Example User Data Script (userdata.sh)

```bash
#!/bin/bash
# Example: Installing IIS on Windows EC2
Install-WindowsFeature -name Web-Server -IncludeManagementTools
echo "<h1>Terraform Project Server 1</h1><p>Instance ID: i-09e2885e2aaac897b</p><p>Welcome to Server 1</p>" > C:\inetpub\wwwroot\index.html
```
## 📜 Example User Data Script (userdata1.sh)
```bash
#!/bin/bash
# Example: Installing IIS on Windows EC2
Install-WindowsFeature -name Web-Server -IncludeManagementTools
echo "<h1>Terraform Project Server 1</h1><p>Instance ID: i-03aaf7ed35d542e02</p><p>Welcome to Server 2</p>" > C:\inetpub\wwwroot\index.html
```
---

## 🧱 Terraform Resources Created

- AWS VPC with public subnets.
- EC2 Instances with User Data.
- Application Load Balancer (ALB).
- Target Groups and Listener Rules.
- Security Groups allowing HTTP (port 80).
- Outputs for ALB DNS Name.

---

## ⚡ Testing

After deployment, visit the ALB DNS in your browser:
```text
http://myalb-xxxxxxx.us-east-1.elb.amazonaws.com
```

You should see a web page like this:

```
Terraform Project Server 1
Instance ID: i-09e2885e2aaac897b
Welcome to Server 1
```

```
Terraform Project Server 2
Instance ID: i-03aaf7ed35d542e02
Welcome to Server 2
```

---

## 🧹 Clean Up

To remove all created resources and avoid charges:
```bash
terraform destroy
```

---

## 📚 Notes

- Ensure your user data script works for Windows instances.
- Security Group must allow inbound port 80 (HTTP).
- Monitor Target Group Health Status in AWS Console → EC2 → Target Groups → Targets.
- If receiving 502 Bad Gateway:
    - Check IIS is installed and running.
    - Confirm Windows Firewall allows HTTP.
    - Validate ELB health check path is `/`.

---

## 🛠️ Author

Sarfraj Khan

---

## ⚠️ Disclaimer

This setup is for learning and demonstration purposes. Review AWS usage and costs before deploying in production.
