# 🔐 Lab 1: Introduction to AWS IAM

## 📌 Overview

AWS Identity and Access Management (IAM) is a web service that enables Amazon Web Services (AWS) customers to manage users and user permissions in AWS.  

With IAM, you can centrally manage:
- Users  
- Security credentials (passwords, access keys, MFA)  
- Permissions controlling access to AWS resources  

---

## 🎯 Lab Objectives

This lab demonstrates:

- Exploring pre-created IAM Users and Groups  
- Inspecting IAM Policies  
- Assigning users to groups  
- Using IAM sign-in URL  
- Testing access control  

---

## ⚠️ AWS Service Restrictions

In this lab environment:
- Some services are restricted  
- Unauthorized errors may appear  
- Only required services are accessible  

---

## 🧠 IAM Concepts

IAM allows you to:

- Manage Users and permissions  
- Manage Roles (temporary access)  
- Enable federated access (SSO)  

---

## ⏱️ Duration

⏳ ~40 minutes

---

## 🚀 Accessing AWS Console

### Step 1: Start Lab

- Click **Start Lab**
- Wait until status is **green**

---

### Step 2: Open AWS Console

- Click **AWS link**
- Allow pop-ups if blocked

---

### Step 3: Workspace Setup

- Arrange:
  - Lab instructions  
  - AWS Console  

---

## 🛠️ Task 1: Explore Users and Groups

### Step 1: Open IAM

- Go to **Services**
- Search **IAM**

<p align="center">
  <img src="./screenshots/iam-dashboard.png" width="700"/>
</p>
<p align="center">
  <em>Figure 1: IAM Dashboard</em>
</p>

---

### Step 2: View Users

- Click **Users**
- Available users:
  - user-1  
  - user-2  
  - user-3  

<p align="center">
  <img src="./screenshots/iam-users.png" width="700"/>
</p>
<p align="center">
  <em>Figure 2: IAM Users List</em>
</p>

---

### Step 3: Inspect user-1

- Permissions → ❌ None  
- Groups → ❌ None  
- Security Credentials → ✅ Password  

📌 Conclusion: No permissions assigned yet

---

### Step 4: Explore Groups

- Go to **User Groups**

Groups:
- EC2-Admin  
- EC2-Support  
- S3-Support  

<p align="center">
  <img src="./screenshots/iam-groups.png" width="700"/>
</p>
<p align="center">
  <em>Figure 3: IAM Groups</em>
</p>

---

### Step 5: Analyze EC2-Support Policy

- Open **EC2-Support**
- Go to **Permissions**
- Policy: `AmazonEC2ReadOnlyAccess`

<p align="center">
  <img src="./screenshots/iam-policies.png" width="700"/>
</p>
<p align="center">
  <em>Figure 4: EC2 ReadOnly Policy</em>
</p>

✔️ Allows:
- Describe EC2  
- List resources  

❌ Denies:
- Start / Stop instances  

---

### Step 6: Analyze S3-Support Policy

- Policy: `AmazonS3ReadOnlyAccess`

✔️ Allows:
- Get objects  
- List buckets  

---

### Step 7: Analyze EC2-Admin Policy

- Inline policy

✔️ Allows:
- Describe EC2  
- Start / Stop instances  

---

## 🧑‍💼 Task 2: Add Users to Groups

### 🎯 Scenario

| User   | Group         | Role |
|--------|--------------|------|
| user-1 | S3-Support   | S3 Support |
| user-2 | EC2-Support  | EC2 Support |
| user-3 | EC2-Admin    | EC2 Admin |

---

### Step 1: Add user-1

- Go to **S3-Support**
- Users tab → Add users  
- Select `user-1`

<p align="center">
  <img src="./screenshots/add-users.png" width="700"/>
</p>
<p align="center">
  <em>Figure 5: Add user-1 to S3-Support</em>
</p>

---

### Step 2: Add user-2

- Assign to **EC2-Support**

---

### Step 3: Add user-3

- Assign to **EC2-Admin**

---

### Step 4: Verification

- Each group must contain **1 user**

---

## 🔐 Task 3: Test User Access

### Step 1: Get Sign-in URL

- Go to **IAM Dashboard**
- Copy login URL

---

### Step 2: Open Incognito Window

- Chrome → New Incognito  
- Firefox → Private Window  

---

## 🧪 Test user-1

Login:
- user-1 / Lab-Password1  

---

### S3 Access

<p align="center">
  <img src="./screenshots/s3-access.png" width="700"/>
</p>
<p align="center">
  <em>Figure 6: user-1 S3 Access</em>
</p>

✔️ Allowed  

---

### EC2 Access

<p align="center">
  <img src="./screenshots/ec2-denied.png" width="700"/>
</p>
<p align="center">
  <em>Figure 7: user-1 EC2 Denied</em>
</p>

❌ Not authorized  

---

## 🧪 Test user-2

Login:
- user-2 / Lab-Password2  

---

### EC2 Read Only

<p align="center">
  <img src="./screenshots/ec2-readonly.png" width="700"/>
</p>
<p align="center">
  <em>Figure 8: user-2 EC2 ReadOnly</em>
</p>

---

### Stop Instance

<p align="center">
  <img src="./screenshots/stop-denied.png" width="700"/>
</p>
<p align="center">
  <em>Figure 9: Stop Instance Denied</em>
</p>

❌ Denied  

---

### S3 Access

❌ No access  

---

## 🧪 Test user-3

Login:
- user-3 / Lab-Password3  

---

### EC2 Full Access

<p align="center">
  <img src="./screenshots/ec2-admin.png" width="700"/>
</p>
<p align="center">
  <em>Figure 10: EC2 Admin Access</em>
</p>

✔️ Can Stop Instance  

---

## 📤 Submitting the Lab

- Click **Submit**
- Wait a few minutes  
- Check **Grades**

📌 Tip:
- Retry submission if needed  

---

## 🧾 Conclusion

You have successfully:

- Explored IAM Users & Groups  
- Analyzed IAM Policies  
- Implemented RBAC (Role-Based Access Control)  
- Tested real access scenarios  
- Understood permission boundaries  

---

## 🔐 Security Best Practices

- Use Least Privilege  
- Use Groups for permissions  
- Enable MFA  
- Avoid root account usage  

---

## 📌 Status

✅ Completed
