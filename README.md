# AWS 3-Tier Architecture with Automated Jenkins Log Backup

## 🚀 Project Overview
This project demonstrates an enterprise-grade, highly available, and secure infrastructure deployed on **Amazon Web Services (AWS)** paired with a **Jenkins CI/CD automation pipeline** that securely backs up build logs to Amazon S3.

---

## 🛠️ Architecture & Tech Stack
* **Cloud Provider:** AWS (Amazon Web Services)
* **Compute:** EC2, Launch Templates, Auto Scaling Groups (ASG)
* **Networking:** VPC, Public/Private Subnets across multiple Availability Zones, Internet Gateway, NAT Gateway, S3 Gateway Endpoint
* **Database:** Amazon RDS (MySQL) in private subnets with restricted security group chaining
* **CI/CD & Automation:** Jenkins, AWS CLI, Bash Scripting
* **Storage:** Amazon S3

---

## 🏗️ Architecture Breakdown
1. **Web Tier (Presentation):** Hosted in public subnets behind an Internet-facing Application Load Balancer, running Apache HTTP server configured via EC2 User Data.
2. **Application Tier (Logic):** Hosted in private subnets behind an Internal Application Load Balancer. Communicates via custom security group rules and ICMP.
3. **Database Tier (Data):** Managed MySQL database (`db.t2.micro`) isolated in private subnets with a custom DB Subnet Group, allowing access *only* from the application security group on port 3306.
4. **DevOps Automation:** A dedicated Jenkins server that runs an automated bash script (`s3upload.sh`) to parse, filter, and upload daily build logs directly to an encrypted Amazon S3 bucket.

---

## 📜 Automation Script (`s3upload.sh`)
The custom shell script iterates through Jenkins job and build directories, checks if logs were created on the current date, and securely pushes them to S3.
