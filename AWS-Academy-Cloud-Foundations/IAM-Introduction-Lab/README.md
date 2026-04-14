# 🔐 Lab 1: Introduction to AWS IAM

## 📌 Overview

AWS Identity and Access Management (IAM) is a core AWS service that enables secure control of access to AWS resources.  
It allows you to manage users, groups, roles, and permissions in a centralized way.

---

## 🎯 Objectives

In this lab, you will:

- Explore pre-created IAM Users and Groups  
- Understand IAM Policies and permissions  
- Assign users to groups based on roles  
- Test access control using different IAM users  
- Learn how permissions impact AWS service access  

---

## ⏱️ Duration

⏳ Approx. **40 minutes**

---

## 🧠 Key Concepts

- **IAM User**: Represents an individual with login credentials  
- **IAM Group**: Collection of users with shared permissions  
- **IAM Role**: Temporary access for users or AWS services  
- **Policy**: JSON document defining permissions (Allow / Deny)  
- **MFA**: Multi-Factor Authentication for enhanced security  

---

## 🏗️ IAM Architecture Diagram

<p align="center">
  <img src="./screenshots/iam-architecture.png" alt="IAM Architecture" width="700"/>
</p>

---

## 📌 Architecture Explanation

- `user-1` → assigned to **S3-Support**  
- `user-2` → assigned to **EC2-Support**  
- `user-3` → assigned to **EC2-Admin**  

---

## 🛠️ Tasks & Implementation

### 🔍 Task 1: Explore Users and Groups

📸 IAM Dashboard
<p align="center">
  <img src="./screenshots/iam-dashboard.png" width="700"/>
</p>

📸 IAM Users
<p align="center">
  <img src="./screenshots/iam-users.png" width="700"/>
</p>

📸 IAM Groups
<p align="center">
  <img src="./screenshots/iam-groups.png" width="700"/>
</p>

📸 IAM Policies
<p align="center">
  <img src="./screenshots/iam-policies.png" width="700"/>
</p>

---

### 🧑‍💼 Task 2: Add Users to Groups

📸 Add Users to Groups
<p align="center">
  <img src="./screenshots/add-users.png" width="700"/>
</p>

---

### 🔐 Task 3: Test User Access

#### 🔹 user-1 (S3 Support)

📸 S3 Access
<p align="center">
  <img src="./screenshots/s3-access.png" width="700"/>
</p>

📸 EC2 Access Denied
<p align="center">
  <img src="./screenshots/ec2-denied.png" width="700"/>
</p>

---

#### 🔹 user-2 (EC2 Support)

📸 EC2 Read-Only
<p align="center">
  <img src="./screenshots/ec2-readonly.png" width="700"/>
</p>

📸 Stop Instance Denied
<p align="center">
  <img src="./screenshots/stop-denied.png" width="700"/>
</p>

---

#### 🔹 user-3 (EC2 Admin)

📸 EC2 Admin Access
<p align="center">
  <img src="./screenshots/ec2-admin.png" width="700"/>
</p>

---

## 🔐 Security Best Practices

- Apply **Least Privilege Principle**  
- Use **Groups instead of individual permissions**  
- Enable **MFA for all users**  

---

## 📚 What I Learned

- IAM access control  
- Policy-based permissions  
- Real-world RBAC implementation  

---

## 📌 Status

✅ Completed
