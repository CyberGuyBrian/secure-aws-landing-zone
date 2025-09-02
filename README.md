# 🛡️ Secure AWS Landing Zone (Terraform)

A production-style **AWS secure landing zone** built with Terraform.  
This project creates a baseline AWS environment that enforces **security, logging, monitoring, and compliance best practices** — the foundation most real companies require before deploying workloads.

---

## 🚀 Features

- **Networking**
  - VPC with public + private subnets across two Availability Zones
  - Internet + NAT Gateways for secure outbound traffic
  - Bastion host in public subnet for admin access

- **Logging & Monitoring**
  - Centralized **S3 log bucket** (encrypted, versioned, blocked public access)
  - **CloudTrail** enabled (multi-region, log validation)
  - **CloudWatch alarm** on root login attempts and failed logins

- **Security & Compliance**
  - **GuardDuty, Config, and Security Hub** enabled by default
  - IAM roles: least-privilege Terraform role + Read-Only viewer role
  - Default encryption with **KMS / AES-256**
  - Aligned with **CIS AWS Foundations Benchmark**

- **Infrastructure as Code**
  - 100% Terraform managed
  - Modular structure for reusability
  - Easy deployment and teardown

---

## 🏗️ Architecture

Internet
   |
[IGW]        +---------------------------+
   |         |        VPC 10.0.0.0/16    |
Public RT -->|  Public  |   Public       |
             | 10.0.1.0 |   10.0.3.0     |
             |   [Bastion EC2]           |
             |  Private |   Private      |
             | 10.0.2.0 |   10.0.4.0     |
             |   (App)      (DB)         |
             +---------------------------+
          [NAT GW] -> outbound only from private

Logs: S3 (encrypted, versioned)  
Audit: CloudTrail → S3  
Detect: GuardDuty + Security Hub  
Config: Record all resources  

---

## 🔧 Prerequisites

- AWS account (Free Tier is sufficient)  
- IAM user with programmatic access  
- [Terraform v1.6+](https://developer.hashicorp.com/terraform/downloads)  
- AWS CLI configured (`aws configure`)  
- SSH keypair for bastion access (`~/.ssh/id_rsa.pub`)  

---

## 📦 Deployment

```bash
# Navigate to project
cd terraform

# Initialize
terraform init

# Preview changes
terraform plan -out tfplan

# Apply
terraform apply tfplan

# Teardown (to avoid charges)
terraform destroy
```

For convenience, a Makefile is included:

```bash
make deploy
make destroy
```

---

## 🔒 Security Choices

- Encryption everywhere (S3 AES-256, CloudTrail log validation)  
- Least privilege IAM (Terraform role + viewer role)  
- No public access to logs (blocked at bucket level)  
- Automated detection with GuardDuty, Config, Security Hub  
- CloudWatch alarms for root login failures  

---

## 📈 Resume / Portfolio Value

This project demonstrates:  
- Designing a secure AWS VPC architecture  
- Automating with Terraform (Infrastructure as Code)  
- Implementing logging, monitoring, and security controls  
- Aligning with real-world compliance frameworks (CIS, NIST, ISO 27001)  

**Example résumé bullet:**  
Built a secure AWS landing zone with Terraform: multi-AZ VPC, IAM least-privilege roles, encrypted S3 log bucket, CloudTrail, GuardDuty, Config, and Security Hub enabled by default; CloudWatch alarms configured for critical events.

---

## 🔮 Future Enhancements

- Replace bastion host with SSM Session Manager (no SSH needed)  
- Add Application Load Balancer + WAF for web app protection  
- Expand into multi-account landing zone with AWS Organizations  
- Integrate with Splunk or Datadog for SIEM analytics  

---

## 📷 Screenshots (Optional)

- CloudTrail log bucket in S3  
- GuardDuty findings dashboard  
- Security Hub CIS benchmark results  
- CloudWatch alarms in action  

---

## 📜 License

This project is licensed under the MIT License. See LICENSE for details.