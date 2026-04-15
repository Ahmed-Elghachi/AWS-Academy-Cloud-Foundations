# 🌐 AWS Lab 2 – Build Your VPC and Launch a Web Server

---

## 📌 Lab Overview

In this lab, you will build a complete AWS network infrastructure and deploy a web server inside it.

### 🔧 Services Used
- Amazon VPC
- Amazon EC2
- Internet Gateway
- NAT Gateway
- Security Groups

---

## 🎯 Objectives

- Create a VPC  
- Create Public & Private Subnets  
- Configure Route Tables  
- Create a Security Group  
- Launch an EC2 Web Server  

---

## 🏗️ Architecture

<p align="center">
  <img src="./screenshots/vpc-architecture.png" width="800"/>
</p>

<p align="center">
  <em>Figure 1: Final VPC Architecture</em>
</p>

---

# 🌐 Task 1: Create Your VPC

## Step 1: Open VPC Console

- Go to AWS Console  
- Search for **VPC**  
- Select **VPC Dashboard**

---

## Step 2: Create VPC

- Click **Create VPC**
- Choose **VPC and more**

### Configuration:

- Name: `lab-vpc`
- CIDR: `10.0.0.0/16`
- Availability Zones: `1`
- Public Subnets: `1`
- Private Subnets: `1`

### Subnet CIDR:

- Public: `10.0.0.0/24`
- Private: `10.0.1.0/24`

### Additional Settings:

- NAT Gateway: **Enabled**
- VPC Endpoints: **None**
- DNS Hostnames: **Enabled**
- DNS Resolution: **Enabled**

---

## 📸 Screenshot

<p align="center">
  <img src="./screenshots/create-vpc.png" width="700"/>
</p>

<p align="center">
  <em>Figure 2: Create VPC Configuration</em>
</p>

---

## 🧠 Explanation

- `/16` → Large network range  
- Public subnet → Internet access  
- Private subnet → Internal use only  
- NAT Gateway → Secure internet access for private subnet  

---

# 🌍 Task 2: Create Additional Subnets

## Step 1: Create Public Subnet 2

- Go to **Subnets**
- Click **Create subnet**

### Configuration:

- VPC: `lab-vpc`
- Name: `lab-subnet-public2`
- AZ: `us-east-1b`
- CIDR: `10.0.2.0/24`

---

## Step 2: Create Private Subnet 2

- Click **Create subnet**

### Configuration:

- VPC: `lab-vpc`
- Name: `lab-subnet-private2`
- AZ: `us-east-1b`
- CIDR: `10.0.3.0/24`

---

## 📸 Screenshot

<p align="center">
  <img src="./screenshots/subnets.png" width="700"/>
</p>

<p align="center">
  <em>Figure 3: Subnets Creation</em>
</p>

---

# 🔀 Configure Route Tables

## Step 1: Private Route Table

- Go to **Route Tables**
- Select `lab-rtb-private1-us-east-1a`

### Check Routes:

- Destination: `0.0.0.0/0`
- Target: NAT Gateway

---

## Step 2: Associate Private Subnets

- Go to **Subnet Associations**
- Add:
  - `lab-subnet-private1`
  - `lab-subnet-private2`

---

## Step 3: Public Route Table

- Select `lab-rtb-public`

### Check Routes:

- Destination: `0.0.0.0/0`
- Target: Internet Gateway

---

## Step 4: Associate Public Subnets

- Add:
  - `lab-subnet-public1`
  - `lab-subnet-public2`

---

## 📸 Screenshot

<p align="center">
  <img src="./screenshots/route-tables.png" width="700"/>
</p>

<p align="center">
  <em>Figure 4: Route Tables Configuration</em>
</p>

---

## 🧠 Explanation

- Public subnet → direct internet access  
- Private subnet → internet via NAT (secure)  

---

# 🔐 Task 3: Create Security Group

## Step 1: Create Security Group

- Go to **Security Groups**
- Click **Create security group**

### Configuration:

- Name: `Web Security Group`
- Description: `Enable HTTP access`
- VPC: `lab-vpc`

---

## Step 2: Add Rule

- Type: HTTP  
- Port: 80  
- Source: `0.0.0.0/0`

---

## 📸 Screenshot

<p align="center">
  <img src="./screenshots/security-group.png" width="700"/>
</p>

<p align="center">
  <em>Figure 5: Security Group</em>
</p>

---

## 🧠 Explanation

- Acts as a firewall  
- Allows web traffic only  

---

# 🖥️ Task 4: Launch EC2 Instance

## Step 1: Launch Instance

- Go to **EC2**
- Click **Launch Instance**

---

## Step 2: Configure Instance

- Name: `Web Server 1`
- AMI: Amazon Linux 2023
- Instance Type: `t2.micro`
- Key Pair: `vockey`

---

## Step 3: Network Settings

- VPC: `lab-vpc`
- Subnet: `lab-subnet-public2`
- Auto Public IP: **Enabled**
- Security Group: **Web Security Group**

---

## 📸 Screenshot

<p align="center">
  <img src="./screenshots/ec2.png" width="700"/>
</p>

<p align="center">
  <em>Figure 6: EC2 Configuration</em>
</p>

---

# ⚙️ User Data Script

```bash
#!/bin/bash

# Install Apache Web Server
dnf install -y httpd wget php mariadb105-server

# Download application
wget https://aws-tc-largeobjects.s3.us-west-2.amazonaws.com/CUR-TF-100-ACCLFO-2/2-lab2-vpc/s3/lab-app.zip

# Extract files
unzip lab-app.zip -d /var/www/html/

# Enable Apache
chkconfig httpd on

# Start Apache
service httpd start
