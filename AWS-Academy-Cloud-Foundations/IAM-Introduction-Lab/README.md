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

<p align="center">
  <em>Figure: IAM Users, Groups and Policies mapping</em>
</p>

---

## 📌 Architecture Explanation

- `user-1` → assigned to **S3-Support** (Read-only S3 access)  
- `user-2` → assigned to **EC2-Support** (Read-only EC2 access)  
- `user-3` → assigned to **EC2-Admin** (Start/Stop EC2 instances)  

👉 This demonstrates **Role-Based Access Control (RBAC)** using IAM groups.

---

## 🛠️ Tasks & Implementation

### 🔍 Task 1: Explore Users and Groups

- Navigate to IAM Dashboard  

📸 **IAM Dashboard**
![IAM Dashboard](./screenshots/iam-dashboard.png)

- View existing users:
  - `user-1`
  - `user-2`
  - `user-3`

📸 **IAM Users**
![IAM Users](./screenshots/iam-users.png)

- Explore IAM Groups:
  - `EC2-Admin`
  - `EC2-Support`
  - `S3-Support`

📸 **IAM Groups**
![IAM Groups](./screenshots/iam-groups.png)

- Analyze attached policies:

📸 **Policy Details**
![IAM Policies](./screenshots/iam-policies.png)

---

### 🧑‍💼 Task 2: Add Users to Groups

| User   | Group         | Permissions |
|--------|--------------|------------|
| user-1 | S3-Support   | Read-only S3 |
| user-2 | EC2-Support  | Read-only EC2 |
| user-3 | EC2-Admin    | Start/Stop EC2 |

#### Steps:

- Add `user-1` → **S3-Support**
- Add `user-2` → **EC2-Support**
- Add `user-3` → **EC2-Admin**

📸 **Add Users to Groups**
![Add Users](./screenshots/add-users.png)

---

### 🔐 Task 3: Test User Access

#### 🔹 Test user-1 (S3 Support)

- Access **S3 → ✅ Allowed**  

📸 **S3 Access**
![S3 Access](./screenshots/s3-access.png)

- Access **EC2 → ❌ Denied**

📸 **EC2 Denied**
![EC2 Denied](./screenshots/ec2-denied.png)

---

#### 🔹 Test user-2 (EC2 Support)

- Access **EC2 → 👁️ Read-only allowed**

📸 **EC2 Read Only**
![EC2 Read Only](./screenshots/ec2-readonly.png)

- Try stopping instance → ❌ Denied  

📸 **Stop Instance Denied**
![Stop Denied](./screenshots/stop-denied.png)

---

#### 🔹 Test user-3 (EC2 Admin)

- Start/Stop instance → ✅ Allowed  

📸 **EC2 Admin Access**
![EC2 Admin](./screenshots/ec2-admin.png)

---

## 🔐 Security Best Practices

- Apply **Least Privilege Principle**  
- Use **Groups instead of individual permissions**  
- Enable **MFA for all users**  
- Avoid using root account  
- Use **roles for temporary access**  

---

## ⚠️ Common Issues

- ❌ Access denied → Check IAM policies  
- ❌ Cannot see EC2 → Check selected Region  
- ❌ Login issues → Verify credentials  

---

## 📸 Screenshots Structure

Make sure your repo contains:
