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
                           ┌─────────────────────┐
                           │      👤 User        │
                           │ AWS Management GUI  │
                           └─────────┬───────────┘
                                     │
                                     ▼
                     ┌──────────────────────────┐
                     │ ☁️ AWS Cloud Environment │
                     └────────────┬─────────────┘
                                  │
                                  ▼
                    ┌───────────────────────────┐
                    │ 🖥️ EC2 Instance (Lab)     │
                    │ Amazon Linux              │
                    └──────────┬────────────────┘
                               │
                ┌──────────────┴──────────────┐
                ▼                             ▼
      ┌──────────────────┐         ┌──────────────────┐
      │ 📦 EBS Volume     │         │ 📸 EBS Snapshot  │
      │ "My Volume"       │────────▶│ "My Snapshot"    │
      │ /dev/sdb          │         │ Stored in S3     │
      └─────────┬────────┘         └─────────┬────────┘
                │                            │
                │                            ▼
                │               ┌────────────────────────┐
                │               │ 📦 Restored Volume     │
                │               │ "Restored Volume"      │
                │               │ /dev/sdc               │
                │               └──────────┬─────────────┘
                │                          │
                └──────────────────────────┘
                               │
                               ▼
                 ┌─────────────────────────┐
                 │ 📁 Mounted File Systems │
                 │ /mnt/data-store         │
                 │ /mnt/data-store2        │
                 └─────────────────────────┘
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

### Figure 1 — AWS Lab Environment

```md
![Figure 1 - AWS Lab Environment](screenshots/aws-lab-start.png)
```

---

# 🧩 Task 1 — Create a New EBS Volume

## Step 1: Open EC2

- Go to AWS Console
- Search for **EC2**

### Figure 2 — Open EC2 Service

```md
![Figure 2 - Open EC2 Service](screenshots/open-ec2.png)
```

---

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

### Figure 3 — EC2 Lab Instance

```md
![Figure 3 - EC2 Lab Instance](screenshots/ec2-instance.png)
```

---

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

### Figure 4 — Create EBS Volume

```md
![Figure 4 - Create EBS Volume](screenshots/create-ebs-volume.png)
```

---

### Add Tag

| Key | Value |
|---|---|
| Name | My Volume |

### Figure 5 — Add Volume Tag

```md
![Figure 5 - Add Volume Tag](screenshots/ebs-tag.png)
```

---

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

### Figure 6 — Attach EBS Volume

```md
![Figure 6 - Attach EBS Volume](screenshots/attach-volume.png)
```

---

3. Configure:

| Setting | Value |
|---|---|
| Instance | Lab |
| Device Name | /dev/sdb |

### Figure 7 — Volume Attachment Configuration

```md
![Figure 7 - Volume Attachment Configuration](screenshots/volume-attachment-settings.png)
```

---

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

### Figure 8 — Session Manager Connection

```md
![Figure 8 - Session Manager Connection](screenshots/session-manager.png)
```

---

## Switch User

```bash
sudo su -l ec2-user
```

### Figure 9 — Switch to ec2-user

```md
![Figure 9 - Switch to ec2-user](screenshots/switch-user.png)
```

---

# 🗂️ Task 4 — Create and Configure File System

## Check Existing Storage

```bash
df -h
```

### Figure 10 — Check Existing Storage

```md
![Figure 10 - Check Existing Storage](screenshots/df-h-before.png)
```

---

## Create ext3 Filesystem

```bash
sudo mkfs -t ext3 /dev/sdb
```

### Figure 11 — Create ext3 Filesystem

```md
![Figure 11 - Create ext3 Filesystem](screenshots/mkfs-ext3.png)
```

---

## Create Mount Directory

```bash
sudo mkdir /mnt/data-store
```

### Figure 12 — Create Mount Directory

```md
![Figure 12 - Create Mount Directory](screenshots/mkdir-data-store.png)
```

---

## Mount the Volume

```bash
sudo mount /dev/sdb /mnt/data-store
```

### Figure 13 — Mount EBS Volume

```md
![Figure 13 - Mount EBS Volume](screenshots/mount-volume.png)
```

---

## Configure Auto-Mount

```bash
echo "/dev/sdb   /mnt/data-store ext3 defaults,noatime 1 2" | sudo tee -a /etc/fstab
```

### Figure 14 — Configure fstab

```md
![Figure 14 - Configure fstab](screenshots/configure-fstab.png)
```

---

## Verify fstab

```bash
cat /etc/fstab
```

### Figure 15 — Verify fstab Configuration

```md
![Figure 15 - Verify fstab Configuration](screenshots/verify-fstab.png)
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

### Figure 16 — Verify Mounted Storage

```md
![Figure 16 - Verify Mounted Storage](screenshots/df-h-after.png)
```

---

## Create Test File

```bash
sudo sh -c "echo some text has been written > /mnt/data-store/file.txt"
```

### Figure 17 — Create Test File

```md
![Figure 17 - Create Test File](screenshots/create-test-file.png)
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

### Figure 18 — Verify File Content

```md
![Figure 18 - Verify File Content](screenshots/verify-test-file.png)
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

### Figure 19 — Create Snapshot

```md
![Figure 19 - Create Snapshot](screenshots/create-snapshot.png)
```

---

### Add Tag

| Key | Value |
|---|---|
| Name | My Snapshot |

### Figure 20 — Snapshot Tag Configuration

```md
![Figure 20 - Snapshot Tag Configuration](screenshots/snapshot-tag.png)
```

---

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

### Figure 21 — Snapshot Completed

```md
![Figure 21 - Snapshot Completed](screenshots/snapshot-completed.png)
```

---

## Delete Test File

```bash
sudo rm /mnt/data-store/file.txt
```

### Figure 22 — Delete Test File

```md
![Figure 22 - Delete Test File](screenshots/delete-file.png)
```

---

## Verify Deletion

```bash
ls /mnt/data-store/
```

### Figure 23 — Verify File Deletion

```md
![Figure 23 - Verify File Deletion](screenshots/verify-delete.png)
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

### Figure 24 — Restore Snapshot to New Volume

```md
![Figure 24 - Restore Snapshot to New Volume](screenshots/restore-snapshot.png)
```

---

## Configuration

| Setting | Value |
|---|---|
| Availability Zone | Same as EC2 |

### Add Tag

| Key | Value |
|---|---|
| Name | Restored Volume |

### Figure 25 — Restored Volume Tag

```md
![Figure 25 - Restored Volume Tag](screenshots/restored-volume-tag.png)
```

---

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

### Figure 26 — Attach Restored Volume

```md
![Figure 26 - Attach Restored Volume](screenshots/attach-restored-volume.png)
```

---

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

### Figure 27 — Create Second Mount Directory

```md
![Figure 27 - Create Second Mount Directory](screenshots/create-data-store2.png)
```

---

## Mount Volume

```bash
sudo mount /dev/sdc /mnt/data-store2
```

### Figure 28 — Mount Restored Volume

```md
![Figure 28 - Mount Restored Volume](screenshots/mount-restored-volume.png)
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

### Figure 29 — Verify Restored File

```md
![Figure 29 - Verify Restored File](screenshots/verify-restored-file.png)
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
