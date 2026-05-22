# 🌐 AWS Lab 3 – Introduction to Amazon EC2

---

# 📌 Lab Overview and Objectives

---

# 🧠 Introduction

This lab provides a practical introduction to launching, managing, resizing, and monitoring an Amazon EC2 instance.

Amazon Elastic Compute Cloud (Amazon EC2) is a web service that provides resizable compute capacity in the cloud. It is designed to make web-scale cloud computing easier for developers.

Amazon EC2 provides a simple web service interface that allows users to:
- Launch virtual servers quickly
- Configure computing resources easily
- Scale infrastructure dynamically
- Manage cloud servers efficiently

Amazon EC2 gives you complete control over your computing resources and allows you to run applications on Amazon’s highly reliable cloud infrastructure.

One of the major advantages of Amazon EC2 is its flexibility:
- You can increase or decrease resources at any time
- You only pay for the resources you use
- You can deploy applications within minutes

Amazon EC2 also helps developers build highly available and fault-tolerant applications while reducing infrastructure costs.

---

# 🧠 Architectural Diagram

<p align="center">
  <img src="./screenshots/ec2-architecture.png" width="850"/>
</p>

<p align="center">
  <em>Figure 1: Amazon EC2 Web Server Architecture</em>
</p>

---

# 🎯 Lab Objectives

After completing this lab, you will be able to:

---

## ✅ Launch a Web Server with Termination Protection Enabled

You will create and configure an Amazon EC2 instance running a web server while enabling termination protection to prevent accidental deletion.

---

## ✅ Monitor Your EC2 Instance

You will use monitoring tools such as:
- Status Checks
- CloudWatch Metrics
- System Logs
- Instance Screenshots

to observe the health and performance of your instance.

---

## ✅ Modify the Security Group to Allow HTTP Access

You will configure inbound security group rules to allow web traffic on port 80 (HTTP) so users can access the hosted web application.

---

## ✅ Resize Your Amazon EC2 Instance

You will:
- Change the EC2 instance type
- Increase computing resources
- Modify the EBS storage volume size

This demonstrates how AWS supports scalability.

---

## ✅ Enable and Test Stop Protection

You will enable stop protection to prevent accidental stopping of your EC2 instance and test how this protection works.

---

## ✅ Explore Amazon EC2 Limits

You will use AWS Service Quotas to explore:
- EC2 instance limits
- Running instance quotas
- Resource limitations per region

---

## ✅ Stop Your EC2 Instance

You will safely stop your EC2 instance after disabling stop protection.

---

# 🧠 Technologies Used

- Amazon EC2
- Amazon EBS
- Amazon VPC
- Security Groups
- Amazon CloudWatch
- AWS Service Quotas
- Apache Web Server

---

# 🎓 Skills Acquired

By completing this lab, you will gain practical experience in:

- Cloud Infrastructure Management
- AWS Compute Services
- EC2 Deployment
- Network Security
- Server Monitoring
- Resource Scaling
- Cloud Administration

---
# ⏱️ Duration

Approximate time: 35 minutes

---

# 🌐 Task 1 — Launch Your Amazon EC2 Instance

---

# 📌 Description

In this task, you will launch an Amazon EC2 instance configured as a Web Server.

You will also:
- Enable termination protection
- Enable stop protection
- Configure User Data
- Deploy a simple Apache web server

---

# ⚙️ Step 1 — Open EC2 Console

- Open AWS Management Console
- Choose Services
- Select EC2

Verify region:
- `N. Virginia (us-east-1)`

---

# EC2 Dashboard

<p align="center">
  <img src="./screenshots/ec2-dashboard.png" width="750"/>
</p>

<p align="center">
  <em>Figure 2: Amazon EC2 Dashboard</em>
</p>

---

# ⚙️ Step 2 — Launch Instance

- Click Launch Instance

---

# Launch Instance Wizard

<p align="center">
  <img src="./screenshots/launch-instance.png" width="750"/>
</p>

<p align="center">
  <em>Figure 3: EC2 Launch Instance Wizard</em>
</p>

---

# ⚙️ Step 3 — Configure Name and Tags

Set instance name:

- `Web Server`

AWS automatically creates a tag:

| Key | Value |
|---|---|
| Name | Web Server |

---

# Instance Name Configuration

<p align="center">
  <img src="./screenshots/name-tags.png" width="750"/>
</p>

<p align="center">
  <em>Figure 4: Instance Name and Tags</em>
</p>

---

# ⚙️ Step 4 — Select AMI

Choose:
- Amazon Linux
- Amazon Linux 2023 AMI

---

# Amazon Linux AMI

<p align="center">
  <img src="./screenshots/ami.png" width="750"/>
</p>

<p align="center">
  <em>Figure 5: Amazon Linux 2023 AMI</em>
</p>

---

# ⚙️ Step 5 — Choose Instance Type

Select:
- `t2.micro`

Specifications:
- 1 vCPU
- 1 GiB RAM

---

# Instance Type Selection

<p align="center">
  <img src="./screenshots/instance-type.png" width="750"/>
</p>

<p align="center">
  <em>Figure 6: EC2 Instance Type</em>
</p>

---

# ⚙️ Step 6 — Configure Key Pair

Select:
- `vockey`

The key pair is used for SSH authentication.

---

# Key Pair Configuration

<p align="center">
  <img src="./screenshots/keypair.png" width="750"/>
</p>

<p align="center">
  <em>Figure 7: EC2 Key Pair</em>
</p>

---

# ⚙️ Step 7 — Configure Network Settings

Choose:
- VPC: `Lab VPC`
- Subnet: `PublicSubnet1`

Enable:
- Auto-assign Public IP

---

# 🌐 Configure Security Group

Choose:
- Create security group

Configure:

| Parameter | Value |
|---|---|
| Security Group Name | Web Server security group |
| Description | Security group for my web server |

Delete the default inbound SSH rule.

---

# Network Settings

<p align="center">
  <img src="./screenshots/network-settings.png" width="750"/>
</p>

<p align="center">
  <em>Figure 8: EC2 Network Configuration</em>
</p>

---

# ⚙️ Step 8 — Configure Storage

Keep default settings:

| Volume Type | Size |
|---|---|
| gp3 SSD | 8 GiB |

---

# Storage Configuration

<p align="center">
  <img src="./screenshots/storage.png" width="750"/>
</p>

<p align="center">
  <em>Figure 9: EC2 Storage Configuration</em>
</p>

---

# ⚙️ Step 9 — Advanced Details

Expand:
- Advanced Details

Enable:
- Termination Protection

---

# 🧠 Termination Protection

Termination protection prevents accidental deletion of the EC2 instance.

---

# ⚙️ Step 10 — Configure User Data Script

Paste the following script:

```bash
#!/bin/bash

dnf install -y httpd

systemctl enable httpd

systemctl start httpd

echo '<html><h1>Hello From Your Web Server!</h1></html>' > /var/www/html/index.html
```

---

# 🧠 User Data Explanation

The script:
- Installs Apache Web Server
- Starts Apache service
- Enables Apache on boot
- Creates a simple HTML page

---

# User Data Configuration

<p align="center">
  <img src="./screenshots/user-data.png" width="750"/>
</p>

<p align="center">
  <em>Figure 10: EC2 User Data Script</em>
</p>

---

# ⚙️ Step 11 — Launch the Instance

- Click Launch Instance

You should see:
- Success Message

---

# Launch Success

<p align="center">
  <img src="./screenshots/launch-success.png" width="750"/>
</p>

<p align="center">
  <em>Figure 11: EC2 Launch Success</em>
</p>

---

# ⚙️ Step 12 — Verify Instance Status

Wait until:

| Parameter | Status |
|---|---|
| Instance State | Running |
| Status Checks | 2/2 checks passed |

---

# Running Instance

<p align="center">
  <img src="./screenshots/ec2-running.png" width="750"/>
</p>

<p align="center">
  <em>Figure 12: EC2 Instance Running</em>
</p>

---

# 🌐 Task 2 — Monitor Your Instance

---

# 📌 Status Checks

Open:
- Status checks tab

Verify:
- System reachability
- Instance reachability

Both should pass successfully.

---

# Status Checks

<p align="center">
  <img src="./screenshots/status-checks.png" width="750"/>
</p>

<p align="center">
  <em>Figure 13: EC2 Status Checks</em>
</p>

---

# 📌 Monitoring

Open:
- Monitoring tab

View:
- CPU utilization
- Network traffic
- Disk metrics

These metrics are provided by Amazon CloudWatch.

---

# Monitoring Metrics

<p align="center">
  <img src="./screenshots/monitoring.png" width="750"/>
</p>

<p align="center">
  <em>Figure 14: Amazon CloudWatch Metrics</em>
</p>

---

# 📌 System Log

Open:
- Actions
- Monitor and troubleshoot
- Get system log

Review:
- Apache installation logs
- Boot process logs

---

# System Log

<p align="center">
  <img src="./screenshots/system-log.png" width="750"/>
</p>

<p align="center">
  <em>Figure 15: EC2 System Log</em>
</p>

---

# 📌 Instance Screenshot

Open:
- Actions
- Monitor and troubleshoot
- Get instance screenshot

This allows troubleshooting graphical console issues.

---

# Instance Screenshot

<p align="center">
  <img src="./screenshots/instance-screenshot.png" width="750"/>
</p>

<p align="center">
  <em>Figure 16: EC2 Instance Screenshot</em>
</p>

---

# 🌐 Task 3 — Update Security Group and Access Web Server

---

# 📌 Access the Web Server

Copy:
- Public IPv4 Address

Open it in browser.

Initially:
- Access will fail

Reason:
- HTTP traffic is blocked by Security Group

---

# ⚙️ Modify Security Group

Open:
- Security Groups
- Web Server security group

Add inbound rule:

| Type | Source |
|---|---|
| HTTP | Anywhere-IPv4 |

Save rules.

---

# Security Group HTTP Rule

<p align="center">
  <img src="./screenshots/http-rule.png" width="750"/>
</p>

<p align="center">
  <em>Figure 17: HTTP Inbound Rule</em>
</p>

---

# 🌐 Access the Web Server Again

Refresh the browser.

You should see:

```html
Hello From Your Web Server!
```

---

# Web Server Output

<p align="center">
  <img src="./screenshots/web-server.png" width="750"/>
</p>

<p align="center">
  <em>Figure 18: Apache Web Server Output</em>
</p>

---

# 🌐 Task 4 — Resize Your EC2 Instance

---

# 📌 Stop the Instance

Select:
- Web Server

Choose:
- Instance state
- Stop instance

Wait until:
- Instance State = Stopped

---

# Stop Instance

<p align="center">
  <img src="./screenshots/stop-instance.png" width="750"/>
</p>

<p align="center">
  <em>Figure 19: Stop EC2 Instance</em>
</p>

---

# ⚙️ Change Instance Type

Choose:
- Actions
- Instance settings
- Change instance type

Set:
- `t2.small`

---

# Instance Resize

<p align="center">
  <img src="./screenshots/resize-instance.png" width="750"/>
</p>

<p align="center">
  <em>Figure 20: Change EC2 Instance Type</em>
</p>

---

# ⚙️ Enable Stop Protection

Choose:
- Actions
- Instance settings
- Change stop protection

Enable:
- Stop Protection

---

# Stop Protection

<p align="center">
  <img src="./screenshots/stop-protection.png" width="750"/>
</p>

<p align="center">
  <em>Figure 21: EC2 Stop Protection</em>
</p>

---

# ⚙️ Modify EBS Volume

Open:
- Storage tab

Select:
- Volume ID

Choose:
- Actions
- Modify volume

Change:
- 8 GiB → 10 GiB

---

# Modify EBS Volume

<p align="center">
  <img src="./screenshots/modify-volume.png" width="750"/>
</p>

<p align="center">
  <em>Figure 22: Modify EBS Volume</em>
</p>

---

# ⚙️ Start the Instance

Choose:
- Instance state
- Start instance

The instance now has:
- More RAM
- Larger Storage

---

# Start Instance

<p align="center">
  <img src="./screenshots/start-instance.png" width="750"/>
</p>

<p align="center">
  <em>Figure 23: Start Resized EC2 Instance</em>
</p>

---

# 🌐 Task 5 — Explore EC2 Limits

---

# 📌 Open Service Quotas

Search:
- Service Quotas

Choose:
- Amazon EC2

Search:
- Running On-Demand

Review EC2 service limits.

---

# Service Quotas

<p align="center">
  <img src="./screenshots/service-quotas.png" width="750"/>
</p>

<p align="center">
  <em>Figure 24: Amazon EC2 Service Quotas</em>
</p>

---

# 🌐 Task 6 — Test Stop Protection

---

# 📌 Stop the Instance

Try stopping the instance again.

You should receive:
- Failed to stop instance

Reason:
- Stop protection is enabled

---

# Stop Protection Error

<p align="center">
  <img src="./screenshots/stop-error.png" width="750"/>
</p>

<p align="center">
  <em>Figure 25: Stop Protection Error</em>
</p>

---

# ⚙️ Disable Stop Protection

Choose:
- Actions
- Instance settings
- Change stop protection

Disable:
- Stop Protection

Save changes.

---

# Disable Stop Protection

<p align="center">
  <img src="./screenshots/disable-stop-protection.png" width="750"/>
</p>

<p align="center">
  <em>Figure 26: Disable Stop Protection</em>
</p>

---

# ⚙️ Stop the Instance Successfully

Now stop the instance again.

The instance will stop successfully.

---

# Instance Stopped

<p align="center">
  <img src="./screenshots/instance-stopped.png" width="750"/>
</p>

<p align="center">
  <em>Figure 27: EC2 Instance Stopped</em>
</p>

---

# 🧠 Final Result

You successfully learned how to:

✅ Launch EC2 instances  
✅ Configure security groups  
✅ Deploy Apache web servers  
✅ Monitor EC2 resources  
✅ Resize EC2 instances  
✅ Modify EBS volumes  
✅ Enable termination protection  
✅ Enable stop protection  
✅ Explore EC2 quotas  

---

# 🎓 Conclusion

This lab introduced the core functionalities of Amazon EC2 including:
- Instance deployment
- Monitoring
- Security
- Storage management
- Scaling
- Protection mechanisms

These concepts are fundamental for:
- Cloud Computing
- DevOps
- Cybersecurity
- Infrastructure Administration
- AWS Solutions Architecture

---
