# 🌐 AWS Lab 2 – Build Your VPC and Launch a Web Server

---

## 📌 Lab Overview

This lab demonstrates how to build a complete AWS network using Amazon VPC and deploy a Web Server inside it.

You will create a custom virtual network with public and private subnets, configure routing, and deploy an EC2 instance.

---

## 🧠 Architecture

<p align="center">
  <img src="./screenshots/vpc-architecture.png" width="850"/>
</p>

<p align="center">
  <em>Figure 1: Final Architecture (VPC + Subnets + NAT + IGW)</em>
</p>

---

## 🎯 Objectives

- Create a VPC  
- Create Public & Private Subnets  
- Configure Route Tables  
- Create a Security Group  
- Launch an EC2 Instance  

---

# 🌐 Task 1: Create Your VPC

---

## 🏗️ VPC Configuration

In this step, we create the main network infrastructure using AWS VPC Wizard (**VPC and more** option).

### 🔧 Configuration Details

| Parameter | Value |
|----------|------|
| VPC Name | `lab-vpc` |
| CIDR Block | `10.0.0.0/16` |
| Availability Zones | 1 |
| Public Subnet | `10.0.0.0/24` |
| Private Subnet | `10.0.1.0/24` |

---

## ⚙️ Automatically Created Components

When selecting **"VPC and more"**, AWS automatically provisions the following components:

### 🌍 Internet Gateway (IGW)
- Enables communication between VPC and Internet  
- Attached automatically to the VPC  

### 🔐 NAT Gateway
- Allows instances in **private subnet** to access Internet  
- Prevents direct inbound access (security layer)  

### 🔀 Route Tables
- **Public Route Table** → routes traffic to Internet Gateway  
- **Private Route Table** → routes traffic to NAT Gateway  

---

## 📸 Screenshot – VPC Creation Configuration

<p align="center">
  <img src="./screenshots/create-vpc.png" width="750"/>
</p>

<p align="center">
  <em>Figure 1: Creating VPC (lab-vpc) with public and private subnets</em>
</p>

---

## 🧠 Detailed Explanation

- The CIDR block `10.0.0.0/16` provides a large IP range (65,536 addresses)  
- Public subnet (`10.0.0.0/24`) is used for resources exposed to the Internet  
- Private subnet (`10.0.1.0/24`) is used for secure internal resources  
- Internet Gateway allows direct Internet access  
- NAT Gateway ensures **secure outbound Internet access** from private subnet  

---

## ✅ Result

After creation, AWS automatically builds:

- ✔ VPC (`lab-vpc`)
- ✔ 1 Public Subnet
- ✔ 1 Private Subnet
- ✔ Internet Gateway
- ✔ NAT Gateway
- ✔ Route Tables

👉 The network is now ready for deploying resources (EC2)

---  

---

# 🌍 Task 2: Create Additional Subnets

## Public Subnet 2

- Name: `lab-subnet-public2`
- CIDR: `10.0.2.0/24`
- AZ: `us-east-1b`

## Private Subnet 2

- Name: `lab-subnet-private2`
- CIDR: `10.0.3.0/24`
- AZ: `us-east-1b`

---

## 📸 Screenshot

<p align="center">
  <img src="./screenshots/subnets.png" width="750"/>
</p>

---

# 🔀 Route Tables Configuration

## Public Route Table

| Destination | Target |
|------------|--------|
| 0.0.0.0/0 | Internet Gateway |

## Private Route Table

| Destination | Target |
|------------|--------|
| 0.0.0.0/0 | NAT Gateway |

---

## 📸 Screenshot

<p align="center">
  <img src="./screenshots/route-tables.png" width="750"/>
</p>

---

## 🧠 Explanation

- Public subnet → direct internet  
- Private subnet → NAT (secure internet access)  

---

# 🔐 Task 3: Security Group

## Configuration

- Name: `Web Security Group`
- Rule: HTTP (Port 80)
- Source: `0.0.0.0/0`

---

## 📸 Screenshot

<p align="center">
  <img src="./screenshots/security-group.png" width="750"/>
</p>

---

## 🧠 Explanation

Acts as a firewall allowing only HTTP traffic.

---

# 🖥️ Task 4: Launch EC2 Instance

## Configuration

- Name: `Web Server 1`
- AMI: Amazon Linux 2023
- Type: t2.micro
- Key Pair: vockey

## Network

- VPC: lab-vpc
- Subnet: lab-subnet-public2
- Public IP: Enabled
- Security Group: Web Security Group

---

## 📸 Screenshot

<p align="center">
  <img src="./screenshots/ec2.png" width="750"/>
</p>

---

# ⚙️ User Data Script

```bash
#!/bin/bash

dnf update -y
dnf install -y httpd wget php mariadb105-server unzip

wget https://aws-tc-largeobjects.s3.us-west-2.amazonaws.com/CUR-TF-100-ACCLFO-2/2-lab2-vpc/s3/lab-app.zip

unzip lab-app.zip -d /var/www/html/

systemctl enable httpd
systemctl start httpd
