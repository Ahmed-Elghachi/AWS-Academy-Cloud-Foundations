# 🔐 Lab 1: Introduction to AWS IAM

## 📌 Overview

AWS Identity and Access Management (IAM) is a web service that enables Amazon Web Services (AWS) customers to manage users and user permissions in AWS.  

With IAM, you can centrally manage:
- Users  
- Security credentials (passwords, access keys, MFA)  
- Permissions controlling which AWS resources users can access  

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
- Some AWS services may be restricted  
- You might see "Not authorized" errors  
- Only required services are available  

---

## 🧠 IAM Concepts

IAM allows you to:

- Manage IAM Users and permissions  
- Manage IAM Roles (temporary access)  
- Enable federated access (SSO)  

---

## 🏗️ IAM Architecture Diagram

<p align="center">
  <img src="./screenshots/iam-architecture.png" alt="IAM Architecture" width="700"/>
</p>

<p align="center">
  <em>Figure 1: IAM Users, Groups and Policies mapping</em>
</p>

---

## ⏱️ Duration

⏳ Approx. **40 minutes**

---

## 🚀 Accessing the AWS Management Console

### Step 1: Start Lab
- Click **Start Lab**
- Wait until the indicator turns **green**

---

### Step 2: Open AWS Console
- Click **AWS link**
- Allow pop-ups if blocked

---

### Step 3: Setup Workspace
- Arrange:
  - AWS Console  
  - Lab instructions  

---

## 🛠️ Task 1: Explore Users and Groups

### Step 1: Open IAM
- Go to **Services**
- Search **IAM**

<p align="center">
  <img src="./screenshots/iam-dashboard.png" width="700"/>
</p>
<p align="center">
  <em>Figure 2: IAM Dashboard</em>
</p>

---

### Step 2: View Users
- Click **Users**
- Existing users:
  - user-1  
  - user-2  
  - user-3  

<p align="center">
  <img src="./screenshots/iam-users.png" width="700"/>
</p>
<p align="center">
  <em>Figure 3: IAM Users List</em>
</p>

---

### Step 3: Inspect user-1
- Permissions → ❌ None  
- Groups → ❌ None  
- Security Credentials → ✅ Password  

<p align="center">
  <img src="./screenshots/user1-details.png" width="700"/>
</p>
<p align="center">
  <em>Figure X: IAM user-1 details (Permissions, Groups, Security Credentials)</em>
</p>
---

### Step 4: Explore Groups
- Go to **User Groups** from the left navigation pane  

Groups available:
- EC2-Admin  
- EC2-Support  
- S3-Support  

<p align="center">
  <img src="./screenshots/iam-groups.png" width="700"/>
</p>
<p align="center">
  <em>Figure 4: IAM Groups Overview</em>
</p>

📌 These groups are pre-created to simulate real-world role-based access control.

---

### Step 5: Analyze EC2-Support Policy
- Select **EC2-Support** group  
- Open **Permissions** tab  
- Policy: `AmazonEC2ReadOnlyAccess`  

<p align="center">
  <img src="./screenshots/ec2-support-permissions.png" width="700"/>
</p>
<p align="center">
  <em>Figure 5: EC2-Support Permissions</em>
</p>

<p align="center">
  <em>Figure 6: EC2 ReadOnly Policy (JSON View)</em>
</p>

```json
{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Effect": "Allow",
            "Action": [
                "ec2:Describe*",
                "ec2:GetSecurityGroupsForVpc"
            ],
            "Resource": "*"
        },
        {
            "Effect": "Allow",
            "Action": "elasticloadbalancing:Describe*",
            "Resource": "*"
        },
        {
            "Effect": "Allow",
            "Action": [
                "cloudwatch:ListMetrics",
                "cloudwatch:GetMetricStatistics",
                "cloudwatch:Describe*"
            ],
            "Resource": "*"
        },
        {
            "Effect": "Allow",
            "Action": "autoscaling:Describe*",
            "Resource": "*"
        }
    ]
}


---
📌 **Explanation:**
- This is an **AWS Managed Policy**
- Automatically maintained by AWS
- Applied to multiple users/groups

✔️ **Allows:**
- Describe EC2 instances  
- List resources  
- View CloudWatch metrics  
- View Auto Scaling  

❌ **Cannot:**
- Start instances  
- Stop instances  
- Modify EC2 resources  

👉 Used for **Support / Monitoring roles**

---

### Step 6: Analyze S3-Support Policy
- Select **S3-Support** group  
- Open **Permissions** tab  
- Policy: `AmazonS3ReadOnlyAccess`  

<p align="center">
  <img src="./screenshots/s3-support-permissions.png" width="700"/>
</p>
<p align="center">
  <em>Figure 7: S3-Support Permissions</em>
</p>

<p align="center">
  <img src="./screenshots/s3-policy-details.png" width="700"/>
</p>
<p align="center">
  <em>Figure 8: S3 ReadOnly Policy Details</em>
</p>

✔️ **Allows:**
- List S3 buckets  
- Read (Get) objects  

❌ **Cannot:**
- Upload files  
- Delete objects  
- Modify buckets  

👉 Used for **Storage support teams**

---

### Step 7: Analyze EC2-Admin Policy
- Select **EC2-Admin** group  
- Open **Permissions** tab  

<p align="center">
  <img src="./screenshots/ec2-admin-permissions.png" width="700"/>
</p>
<p align="center">
  <em>Figure 9: EC2-Admin Permissions</em>
</p>

📌 **Important:**
- Uses an **Inline Policy** (not managed)
- Specific to this group only

<p align="center">
  <img src="./screenshots/ec2-admin-policy.png" width="700"/>
</p>
<p align="center">
  <em>Figure 10: EC2 Admin Inline Policy</em>
</p>

✔️ **Allows:**
- Describe instances  
- Start instances  
- Stop instances  

👉 Used for **EC2 Administrators**

---

## 🧑‍💼 Task 2: Add Users to Groups

### 🎯 Scenario

| User   | Group         | Permissions |
|--------|--------------|------------|
| user-1 | S3-Support   | Read-only S3 |
| user-2 | EC2-Support  | Read-only EC2 |
| user-3 | EC2-Admin    | Start/Stop EC2 |

📌 This scenario demonstrates:

- **Role-Based Access Control (RBAC)**  
- **Least Privilege Principle**  

---

### Step 1: Add user-1
- Go to **S3-Support**
- Add `user-1`

<p align="center">
  <img src="./screenshots/add-users.png" width="700"/>
</p>
<p align="center">
  <em>Figure 6: Add user-1 to S3-Support</em>
</p>

---

### Step 2: Add user-2
- Assign to **EC2-Support**

---

### Step 3: Add user-3
- Assign to **EC2-Admin**

---

### Step 4: Verify
- Each group must contain **1 user**

---

## 🔐 Task 3: Test User Access

### Step 1: Get IAM Login URL
- Copy from IAM Dashboard  

---

### Step 2: Open Incognito Window

---

## 🧪 Test user-1

Login:
- user-1 / Lab-Password1  

### S3 Access

<p align="center">
  <img src="./screenshots/s3-access.png" width="700"/>
</p>
<p align="center">
  <em>Figure 7: user-1 accessing S3</em>
</p>

✔️ Allowed  

---

### EC2 Access

<p align="center">
  <img src="./screenshots/ec2-denied.png" width="700"/>
</p>
<p align="center">
  <em>Figure 8: user-1 denied EC2 access</em>
</p>

❌ Denied  

---

## 🧪 Test user-2

Login:
- user-2 / Lab-Password2  

### EC2 Read Only

<p align="center">
  <img src="./screenshots/ec2-readonly.png" width="700"/>
</p>
<p align="center">
  <em>Figure 9: user-2 EC2 ReadOnly</em>
</p>

---

### Stop Instance

<p align="center">
  <img src="./screenshots/stop-denied.png" width="700"/>
</p>
<p align="center">
  <em>Figure 10: Stop instance denied</em>
</p>

---

### S3 Access
❌ Denied  

---

## 🧪 Test user-3

Login:
- user-3 / Lab-Password3  

### EC2 Admin

<p align="center">
  <img src="./screenshots/ec2-admin.png" width="700"/>
</p>
<p align="center">
  <em>Figure 11: EC2 Admin full access</em>
</p>

✔️ Allowed to stop instance  

---

## 📤 Submitting the Lab

- Click **Submit**
- Wait a few minutes  
- Check **Grades**  

---

## 🧾 Conclusion

You have successfully:

- Explored IAM Users & Groups  
- Analyzed IAM Policies  
- Implemented Role-Based Access Control (RBAC)  
- Tested permissions  

---

## 🔐 Security Best Practices

- Use Least Privilege  
- Use Groups  
- Enable MFA  
- Avoid root account  

---

## 📌 Status

✅ Completed
