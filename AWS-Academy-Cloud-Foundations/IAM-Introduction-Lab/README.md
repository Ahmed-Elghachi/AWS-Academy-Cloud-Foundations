## 🛠️ Tasks & Implementation

### 🔍 Task 1: Explore Users and Groups

#### 🎯 Objective:
Understand how IAM users, groups, and policies are structured and linked together.

---

#### 🧭 Step 1: Access IAM Dashboard

- Connect to AWS Management Console  
- In the search bar → type **IAM**  
- Click on IAM service  

<p align="center">
  <img src="./screenshots/iam-dashboard.png" width="700"/>
</p>

---

#### 👥 Step 2: Explore IAM Users

- Go to **Users** (left menu)
- You will find:
  - `user-1`
  - `user-2`
  - `user-3`

👉 Click on **user-1**

- Check **Permissions tab** → No permissions  
- Check **Groups tab** → No group assigned  
- Check **Security credentials** → Console password exists  

<p align="center">
  <img src="./screenshots/iam-users.png" width="700"/>
</p>

📌 **Conclusion:**  
Users alone have no permissions unless assigned via policies or groups.

---

#### 👨‍👩‍👧 Step 3: Explore IAM Groups

- Go to **User Groups**
- Available groups:
  - `EC2-Admin`
  - `EC2-Support`
  - `S3-Support`

<p align="center">
  <img src="./screenshots/iam-groups.png" width="700"/>
</p>

---

#### 📜 Step 4: Analyze Policies

👉 Open **EC2-Support**

- Attached Policy: `AmazonEC2ReadOnlyAccess`

👉 Click to expand policy

<p align="center">
  <img src="./screenshots/iam-policies.png" width="700"/>
</p>

📌 This policy allows:
- `Describe` (view resources)
- `List` (list resources)

❌ Cannot:
- Start / Stop instances

---

👉 Open **S3-Support**

- Policy: `AmazonS3ReadOnlyAccess`
- Permissions:
  - View buckets
  - Read data

---

👉 Open **EC2-Admin**

- Inline Policy:
  - View EC2
  - Start / Stop instances

---

#### 🧠 Key Learning

- Policies define permissions  
- Groups simplify permission management  
- Users inherit permissions from groups  

---

### 🧑‍💼 Task 2: Add Users to Groups

#### 🎯 Objective:
Assign correct permissions using IAM groups (RBAC model)

---

#### 📊 Business Scenario

| User   | Group         | Role |
|--------|--------------|------|
| user-1 | S3-Support   | S3 Support |
| user-2 | EC2-Support  | EC2 Support |
| user-3 | EC2-Admin    | EC2 Admin |

---

#### ⚙️ Step 1: Add user-1 to S3 Group

- Go to **User Groups**
- Click **S3-Support**
- Go to **Users tab**
- Click **Add users**
- Select `user-1`
- Click **Add**

<p align="center">
  <img src="./screenshots/add-users.png" width="700"/>
</p>

---

#### ⚙️ Step 2: Add user-2 to EC2-Support

- Repeat same steps
- Select `user-2`
- Assign to **EC2-Support**

---

#### ⚙️ Step 3: Add user-3 to EC2-Admin

- Repeat steps
- Assign `user-3` to **EC2-Admin**

---

#### ✅ Verification

- Each group should show **1 user**

📌 This confirms:
- Permissions are assigned via groups (not directly)

---

### 🔐 Task 3: Test User Access

#### 🎯 Objective:
Validate how policies affect access to AWS services

---

#### 🌐 Step 1: Get IAM Login URL

- Go to **IAM Dashboard**
- Copy **Sign-in URL**

---

#### 🧪 Step 2: Test user-1 (S3 Support)

- Open **Incognito window**
- Login:
  - user: `user-1`
  - password: `Lab-Password1`

---

👉 Access S3 → ✅ Allowed

<p align="center">
  <img src="./screenshots/s3-access.png" width="700"/>
</p>

👉 Access EC2 → ❌ Denied

<p align="center">
  <img src="./screenshots/ec2-denied.png" width="700"/>
</p>

📌 Reason:
- Only S3 permissions assigned

---

#### 🧪 Step 3: Test user-2 (EC2 Support)

- Login as `user-2`

👉 Access EC2 → 👁️ Read-only

<p align="center">
  <img src="./screenshots/ec2-readonly.png" width="700"/>
</p>

👉 Try Stop Instance → ❌ Denied

<p align="center">
  <img src="./screenshots/stop-denied.png" width="700"/>
</p>

👉 Access S3 → ❌ Denied

📌 Reason:
- Only EC2 ReadOnly permissions

---

#### 🧪 Step 4: Test user-3 (EC2 Admin)

- Login as `user-3`

👉 Access EC2 → ✅ Full access

👉 Stop instance → ✅ Allowed

<p align="center">
  <img src="./screenshots/ec2-admin.png" width="700"/>
</p>

---

#### 🧠 Final Understanding

| User   | Access Result |
|--------|--------------|
| user-1 | S3 only |
| user-2 | EC2 ReadOnly |
| user-3 | EC2 Full Control |

---

## 🎓 Conclusion

- IAM enforces **security and access control**
- Groups simplify management
- Policies define exact permissions
- Testing confirms real-world behavior

---
