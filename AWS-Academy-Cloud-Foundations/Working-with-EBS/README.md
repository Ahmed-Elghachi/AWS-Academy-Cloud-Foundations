# Lab 4: Working with EBS

## 📌 Lab Overview

This lab focuses on **Amazon Elastic Block Store (Amazon EBS)**, a key storage service used with Amazon EC2 instances.

In this lab, you will learn how to:

- Create an Amazon EBS volume
- Attach and mount the volume to an EC2 instance
- Create a filesystem on the volume
- Create a snapshot backup
- Restore the snapshot into a new volume

---

# 🏗️ Architectural Diagram

```text
                 👤 User
                    │
                    ▼
          🌐 AWS Management Console
                    │
                    ▼
          🖥️ Amazon EC2 Instance
                    │
        ┌───────────┴───────────┐
        ▼                       ▼
 📦 EBS Volume            📸 EBS Snapshot
        │                       │
        └──────────► Restored Volume
```

---

# 🎯 Objectives

By the end of this lab, you will be able to:

- Create an Amazon EBS volume
- Attach and mount the volume to an EC2 instance
- Create a snapshot of the volume
- Restore a volume from a snapshot
- Verify stored data after restoration

---

# 📚 Prerequisites

Before starting this lab, you should be familiar with:

- Basic AWS EC2 usage
- Linux command-line basics
- Filesystem management in Linux

---

# ⏱️ Duration

Approximate lab duration: **30 minutes**

---

# 🔒 AWS Service Restrictions

In this lab environment, access is restricted only to the AWS services required for the lab.

Some AWS actions may not be available.

---

# 📖 What is Amazon EBS?

Amazon Elastic Block Store (EBS) provides persistent block-level storage volumes for use with Amazon EC2 instances.

## Key Features

- Persistent storage
- High availability
- Snapshot backups
- Easy scalability
- High durability
- Independent lifecycle from EC2 instances

---

# ⚙️ Amazon EBS Features

| Feature | Description |
|---|---|
| Persistent Storage | Data survives EC2 stop/start |
| High Performance | SSD-backed storage |
| Reliability | Built-in redundancy |
| Snapshots | Backup volumes to Amazon S3 |
| Scalability | 1 GB to 16 TB |
| Easy Restore | Restore from snapshots |

---

# 🚀 Access AWS Console

1. Click **Start Lab**
2. Wait for **Lab Status: Ready**
3. Click **AWS**
4. Open the AWS Management Console

---

# 🧩 Task 1 — Create a New EBS Volume

## Step 1: Open EC2

- Go to AWS Console
- Search for **EC2**

## Step 2: Verify Existing Instance

- Click **Instances**
- Locate the instance named:

```bash
Lab
```

- Note the Availability Zone:

Example:

```bash
us-east-1a
```

## Step 3: Create Volume

Go to:

```bash
EC2 → Volumes → Create Volume
```

### Configuration

| Setting | Value |
|---|---|
| Volume Type | General Purpose SSD (gp2) |
| Size | 1 GiB |
| Availability Zone | Same as EC2 instance |

### Add Tag

| Key | Value |
|---|---|
| Name | My Volume |

Click:

```bash
Create Volume
```

---

# 🔗 Task 2 — Attach Volume to EC2

## Attach the Volume

1. Select:

```bash
My Volume
```

2. Click:

```bash
Actions → Attach Volume
```

3. Configure:

| Setting | Value |
|---|---|
| Instance | Lab |
| Device Name | /dev/sdb |

4. Click:

```bash
Attach Volume
```

---

# 💻 Task 3 — Connect to EC2

## Use Session Manager

1. Go to:

```bash
EC2 → Instances
```

2. Select:

```bash
Lab
```

3. Click:

```bash
Connect
```

4. Choose:

```bash
Session Manager → Connect
```

## Switch User

```bash
sudo su -l ec2-user
```

---

# 🗂️ Task 4 — Create and Configure File System

## Check Existing Storage

```bash
df -h
```

---

## Create ext3 Filesystem

```bash
sudo mkfs -t ext3 /dev/sdb
```

---

## Create Mount Directory

```bash
sudo mkdir /mnt/data-store
```

---

## Mount the Volume

```bash
sudo mount /dev/sdb /mnt/data-store
```

---

## Configure Auto-Mount

```bash
echo "/dev/sdb   /mnt/data-store ext3 defaults,noatime 1 2" | sudo tee -a /etc/fstab
```

---

## Verify fstab

```bash
cat /etc/fstab
```

---

## Verify Storage

```bash
df -h
```

Expected output includes:

```bash
/dev/xvdb
```

---

## Create Test File

```bash
sudo sh -c "echo some text has been written > /mnt/data-store/file.txt"
```

---

## Verify File

```bash
cat /mnt/data-store/file.txt
```

Expected:

```bash
some text has been written
```

---

# 📸 Task 5 — Create an EBS Snapshot

## Create Snapshot

1. Go to:

```bash
EC2 → Volumes
```

2. Select:

```bash
My Volume
```

3. Click:

```bash
Actions → Create Snapshot
```

### Add Tag

| Key | Value |
|---|---|
| Name | My Snapshot |

4. Click:

```bash
Create Snapshot
```

---

## Verify Snapshot

Go to:

```bash
EC2 → Snapshots
```

Status progression:

```bash
Pending → Completed
```

---

## Delete Test File

```bash
sudo rm /mnt/data-store/file.txt
```

---

## Verify Deletion

```bash
ls /mnt/data-store/
```

---

# ♻️ Task 6 — Restore Snapshot

# Create Volume from Snapshot

1. Select:

```bash
My Snapshot
```

2. Click:

```bash
Actions → Create Volume from Snapshot
```

## Configuration

| Setting | Value |
|---|---|
| Availability Zone | Same as EC2 |

### Add Tag

| Key | Value |
|---|---|
| Name | Restored Volume |

Click:

```bash
Create Volume
```

---

# 🔗 Attach Restored Volume

1. Go to:

```bash
EC2 → Volumes
```

2. Select:

```bash
Restored Volume
```

3. Click:

```bash
Actions → Attach Volume
```

## Configuration

| Setting | Value |
|---|---|
| Instance | Lab |
| Device Name | /dev/sdc |

Click:

```bash
Attach Volume
```

---

# 🗂️ Mount Restored Volume

## Create Directory

```bash
sudo mkdir /mnt/data-store2
```

---

## Mount Volume

```bash
sudo mount /dev/sdc /mnt/data-store2
```

---

## Verify Restored File

```bash
ls /mnt/data-store2/
```

Expected:

```bash
file.txt
```

---

# ✅ Conclusion

In this lab, you successfully:

- Created an Amazon EBS volume
- Attached it to an EC2 instance
- Created a filesystem
- Mounted the volume
- Created a snapshot backup
- Restored the snapshot
- Mounted the restored volume
- Verified data recovery

---

# 🧠 Key Commands Summary

```bash
df -h
sudo mkfs -t ext3 /dev/sdb
sudo mkdir /mnt/data-store
sudo mount /dev/sdb /mnt/data-store
cat /etc/fstab
sudo rm /mnt/data-store/file.txt
sudo mkdir /mnt/data-store2
sudo mount /dev/sdc /mnt/data-store2
```

---

# 📌 Final Notes

Amazon EBS is a powerful persistent storage solution for AWS EC2 instances.

Snapshots provide:

- Backup capability
- Disaster recovery
- Volume cloning
- Data migration

---

# 🏁 Lab Complete

Congratulations 🎉

You have completed:

✅ EBS Volume Creation  
✅ Volume Attachment  
✅ Filesystem Configuration  
✅ Snapshot Creation  
✅ Snapshot Restoration  
✅ Data Recovery Verification

---
