# 🌐 AWS Lab 3 – Introduction to Amazon EC2

---

# 📌 Lab Overview and Objectives

## 🧠 Introduction

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

# ⏱️ Duration

Approximate time: 35 minutes

---

# 🌐 Task 1 — Launch Your Amazon EC2 Instance

## 📌 Description

In this task, you will launch an Amazon EC2 instance with:

- Termination Protection
- Stop Protection
- User Data Script
- Apache Web Server

The EC2 instance will host a simple web application accessible from the internet.

---

# ⚙️ Step 1 — Open EC2 Console

- Open AWS Management Console
- Choose ▦ **Services**
- Select **Compute**
- Choose **EC2**

Verify the region:

- `N. Virginia (us-east-1)`

---

# ⚙️ Step 2 — Launch Instance

- Click **Launch instance**
- Select **Launch instance**

---

# ⚙️ Step 3 — Configure Name and Tags

Set the instance name:

- `Web Server`

AWS automatically creates a tag:

| Key | Value |
|------|------|
| Name | Web Server |

Tags help organize AWS resources.

---

# ⚙️ Step 4 — Choose Amazon Machine Image (AMI)

Keep the default selections:

- Amazon Linux
- Amazon Linux 2023 AMI

---

# 🧠 AMI Explanation

An Amazon Machine Image (AMI) provides:

- Operating System
- Software packages
- Storage configuration
- Launch permissions

The AMI acts as the template for the EC2 instance.

---

# ⚙️ Step 5 — Choose Instance Type

Keep default instance type:

- `t2.micro`

Specifications:

- 1 vCPU
- 1 GiB RAM

---

# 🧠 Instance Type Explanation

Amazon EC2 instance types define:

- CPU resources
- Memory capacity
- Storage performance
- Network performance

The `t2.micro` instance is ideal for lightweight workloads and AWS labs.

---

# ⚙️ Step 6 — Configure Key Pair

Select:

- `vockey`

---

# 🧠 Key Pair Explanation

AWS uses public-key cryptography for secure authentication.

The key pair allows:

- Secure SSH access
- Secure instance authentication

In this lab, the key pair is required even if SSH is not used.

---

# ⚙️ Step 7 — Configure Network Settings

Choose:

- VPC: `Lab VPC`
- Subnet: `PublicSubnet1`

Keep:

- Auto-assign Public IP enabled

---

# 🌐 Configure Security Group

Choose:

- Create security group

Configure:

| Parameter | Value |
|------|------|
| Security Group Name | Web Server security group |
| Description | Security group for my web server |

Remove the default inbound SSH rule.

---

# 🧠 Security Group Explanation

A security group acts as a virtual firewall.

It controls:

- Inbound traffic
- Outbound traffic

Currently:

- No inbound rules are configured
- HTTP access will be added later

---

# ⚙️ Step 8 — Configure Storage

Keep default storage settings:

| Volume Type | Size |
|------|------|
| gp3 SSD | 8 GiB |

---

# 🧠 Storage Explanation

Amazon EC2 uses Amazon Elastic Block Store (EBS).

The root volume:

- Stores the operating system
- Stores applications and data

---

# ⚙️ Step 9 — Configure Advanced Details

Expand:

- Advanced details

Enable:

- Termination Protection

---

# 🧠 Termination Protection

Termination protection prevents accidental deletion of the EC2 instance.

When enabled:

- The instance cannot be terminated unless protection is disabled first.

---

# ⚙️ Step 10 — Configure User Data Script

Paste the following script into the **User data** field:

```bash
#!/bin/bash

dnf install -y httpd

systemctl enable httpd

systemctl start httpd

echo '<html><h1>Hello From Your Web Server!</h1></html>' > /var/www/html/index.html
---

---

# ⚙️ Step 11 — Launch the Instance

At the bottom of the **Summary** panel:

- Click **Launch instance**

AWS will begin provisioning the EC2 instance.

You should now see:

- ✅ Success message

---

# Launch Success

<p align="center">
  <img src="./screenshots/launch-success.png" width="750"/>
</p>

<p align="center">
  <em>Figure 10: EC2 Instance Launch Success Message</em>
</p>

---

# ⚙️ Step 12 — View All Instances

After the instance is launched:

- Choose **View all instances**

In the **Instances** list:

- Select `Web Server`

Review the information displayed in the **Details** tab.

The Details section contains important information about:

- Instance type
- Security settings
- Network configuration
- Public IPv4 address
- Public IPv4 DNS
- Instance ID
- VPC and Subnet information

---

# EC2 Instance Details

<p align="center">
  <img src="./screenshots/ec2-details.png" width="750"/>
</p>

<p align="center">
  <em>Figure 11: Amazon EC2 Instance Details</em>
</p>

---

# 🧠 Public IPv4 DNS Explanation

The instance is automatically assigned a:

- Public IPv4 DNS

This DNS name allows communication with the EC2 instance directly from the Internet.

Example:

```text
ec2-44-xxx-xxx-xxx.compute-1.amazonaws.com
```

This public DNS can later be used to:

- Access web applications
- Connect using SSH
- Test hosted services

---

# 📌 Resize the Console Window

To display more instance information:

- Drag the window divider upward

This provides a larger view of:
- Instance details
- Monitoring information
- Networking configuration
- Security settings

---

# ⚙️ Step 13 — Wait for Instance Initialization

At first, the EC2 instance will appear in the following states:

| State | Description |
|---|---|
| Pending | AWS is launching the instance |
| Initializing | Operating system and services are starting |
| Running | The instance is fully operational |

---

# 🧠 Instance State Explanation

### 🔹 Pending
AWS allocates:
- Virtual hardware
- Storage
- Networking resources

### 🔹 Initializing
Amazon Linux boots and:
- Executes the User Data script
- Starts Apache Web Server
- Configures services

### 🔹 Running
The EC2 instance becomes available for:
- Monitoring
- Remote access
- Web hosting

---

# Running EC2 Instance

<p align="center">
  <img src="./screenshots/ec2-running.png" width="750"/>
</p>

<p align="center">
  <em>Figure 12: EC2 Instance Running Successfully</em>
</p>

---

# ✅ Verification

Wait until the instance displays the following status:

| Parameter | Status |
|---|---|
| Instance State | Running |
| Status Checks | 2/2 checks passed |

---

# 🧠 Status Checks Explanation

Amazon EC2 automatically performs two health checks:

| Status Check | Description |
|---|---|
| System Status Check | Verifies AWS physical infrastructure |
| Instance Status Check | Verifies the operating system inside the instance |

When both checks pass:

- The AWS infrastructure is healthy
- The operating system is functioning correctly
- The instance is ready for use

---

# Status Checks Passed

<p align="center">
  <img src="./screenshots/status-checks-passed.png" width="750"/>
</p>

<p align="center">
  <em>Figure 13: EC2 Status Checks Successfully Passed</em>
</p>

---

# 🎉 Congratulations!

You have successfully launched your first Amazon EC2 instance.

The instance is now:

✅ Running successfully  
✅ Connected to the network  
✅ Protected against accidental termination  
✅ Configured with Apache Web Server  
✅ Ready for monitoring and web access  

---

# ✅ Result

You successfully created and configured an Amazon EC2 instance with:

- Amazon Linux 2023
- Apache Web Server
- User Data Automation
- Public IPv4 Connectivity
- Security Group Configuration
- Termination Protection

The environment is now ready for the next tasks in the lab.

---
