# Lab 6: Scale and Load Balance Your Architecture

---

# 📌 Lab Overview

This lab demonstrates how to use:

- Elastic Load Balancing (ELB)
- Auto Scaling
- Amazon CloudWatch

to build a scalable and highly available AWS architecture.

You will create:

- An Amazon Machine Image (AMI)
- An Application Load Balancer
- A Launch Template
- An Auto Scaling Group
- CloudWatch alarms for automatic scaling

---

# 🎯 Objectives

By the end of this lab, you will be able to:

- Create an AMI from an EC2 instance
- Create an Application Load Balancer
- Configure Target Groups
- Create a Launch Template
- Configure an Auto Scaling Group
- Automatically scale EC2 instances
- Monitor infrastructure using CloudWatch

---


# 🔒 AWS Service Restrictions

In this lab environment, access is limited to the AWS services required for the lab.

⚠️ Launching 20 or more EC2 instances may automatically terminate the lab environment.

---

# 📖 What is Elastic Load Balancing?

Elastic Load Balancing automatically distributes incoming application traffic across multiple EC2 instances.

## Benefits

- High availability
- Fault tolerance
- Automatic traffic distribution
- Improved scalability

---

# 📖 What is Auto Scaling?

Amazon EC2 Auto Scaling automatically adjusts the number of EC2 instances based on traffic and resource usage.

## Benefits

- Automatic scaling
- Reduced operational costs
- Better performance
- High availability

---

# 🚀 Access AWS Console

1. Click **Start Lab**
2. Wait until Lab Status becomes Ready
3. Click **AWS**
4. Open the AWS Management Console

---

# 📸 Scenario Overview

<p align="center">
  <img src="./screenshots/start-lab.png" width="900"/>
</p>

<p align="center">
  <em>Figure 1: Initial and Final AWS Infrastructure </em>
</p>

---

# 🧩 Task 1 — Create an AMI for Auto Scaling

## Step 1 — Open EC2 Console

- Open AWS Console
- Search for EC2

---

## Step 2 — Verify Existing Instance

- Open Instances
- Verify that:

```text
Web Server 1
```

shows:

```text
2/2 checks passed
```

---

# 📸 Figure 4 — Web Server Instance

<p align="center">
  <img src="./screenshots/webserver-instance.png" width="900"/>
</p>

<p align="center">
  <em>Figure 4: Web Server 1 Instance</em>
</p>

---

## Step 3 — Create AMI

Go to:

```text
Actions → Image and templates → Create image
```

Configure:

| Setting | Value |
|---|---|
| Image name | WebServerAMI |
| Image description | Lab AMI for Web Server |

Click:

```text
Create image
```

---

# 📸 Figure 5 — Create AMI

<p align="center">
  <img src="./screenshots/create-ami.png" width="900"/>
</p>

<p align="center">
  <em>Figure 5: Create Amazon Machine Image</em>
</p>

---

# 🌐 Task 2 — Create a Load Balancer

## Step 1 — Create Target Group

Go to:

```text
EC2 → Target Groups
```

Click:

```text
Create target group
```

Configuration:

| Setting | Value |
|---|---|
| Target type | Instances |
| Target group name | LabGroup |
| VPC | Lab VPC |

Click:

```text
Next → Create target group
```

---

# 📸 Figure 6 — Create Target Group

<p align="center">
  <img src="./screenshots/create-target-group.png" width="900"/>
</p>

<p align="center">
  <em>Figure 6: Create Target Group</em>
</p>

---

## Step 2 — Create Application Load Balancer

Go to:

```text
EC2 → Load Balancers
```

Click:

```text
Create Load Balancer
```

Select:

```text
Application Load Balancer
```

---

## Configure Load Balancer

| Setting | Value |
|---|---|
| Name | LabELB |
| VPC | Lab VPC |
| Subnet 1 | Public Subnet 1 |
| Subnet 2 | Public Subnet 2 |

Security Group:

```text
Web Security Group
```

Listener:

| Protocol | Port | Target Group |
|---|---|---|
| HTTP | 80 | LabGroup |

Click:

```text
Create load balancer
```

---

# 📸 Figure 7 — Create Load Balancer

<p align="center">
  <img src="./screenshots/create-load-balancer.png" width="950"/>
</p>

<p align="center">
  <em>Figure 7: Application Load Balancer Configuration</em>
</p>

---

# 🚀 Task 3 — Create Launch Template and Auto Scaling Group

## Step 1 — Create Launch Template

Go to:

```text
EC2 → Launch Templates
```

Click:

```text
Create launch template
```

Configuration:

| Setting | Value |
|---|---|
| Launch template name | LabConfig |
| AMI | WebServerAMI |
| Instance type | t2.micro |
| Key pair | vockey |
| Security Group | Web Security Group |

Enable:

```text
Detailed CloudWatch monitoring
```

Click:

```text
Create launch template
```

---

# 📸 Figure 8 — Launch Template Configuration

<p align="center">
  <img src="./screenshots/launch-template.png" width="950"/>
</p>

<p align="center">
  <em>Figure 8: Launch Template Configuration</em>
</p>

---

## Step 2 — Create Auto Scaling Group

From Actions menu:

```text
Create Auto Scaling group
```

Configuration:

| Setting | Value |
|---|---|
| Auto Scaling group name | Lab Auto Scaling Group |
| Launch template | LabConfig |

---

## Configure Network

| Setting | Value |
|---|---|
| VPC | Lab VPC |
| Subnets | Private Subnet 1, Private Subnet 2 |

---

## Attach Load Balancer

| Setting | Value |
|---|---|
| Target Group | LabGroup |

Enable:

```text
Enable group metrics collection within CloudWatch
```

---

## Configure Scaling

| Setting | Value |
|---|---|
| Desired Capacity | 2 |
| Minimum Capacity | 2 |
| Maximum Capacity | 6 |

Scaling Policy:

| Setting | Value |
|---|---|
| Policy Name | LabScalingPolicy |
| Metric | Average CPU Utilization |
| Target Value | 60 |

Click:

```text
Create Auto Scaling group
```

---

# 📸 Figure 9 — Auto Scaling Group

<p align="center">
  <img src="./screenshots/autoscaling-group.png" width="950"/>
</p>

<p align="center">
  <em>Figure 9: Auto Scaling Group Configuration</em>
</p>

---

# ⚖️ Task 4 — Verify Load Balancing

## Verify Instances

Go to:

```text
EC2 → Instances
```

You should see:

```text
Lab Instance
```

running twice.

---

# 📸 Figure 10 — Auto Scaling Instances

<p align="center">
  <img src="./screenshots/autoscaling-instances.png" width="950"/>
</p>

<p align="center">
  <em>Figure 10: Auto Scaling Instances</em>
</p>

---

## Verify Target Health

Go to:

```text
Target Groups → Targets
```

Wait until targets become:

```text
Healthy
```

---

# 📸 Figure 11 — Healthy Targets

<p align="center">
  <img src="./screenshots/healthy-targets.png" width="950"/>
</p>

<p align="center">
  <em>Figure 11: Healthy Target Instances</em>
</p>

---

## Test Load Balancer

Copy the DNS name of:

```text
LabELB
```

Open it in a browser.

The web application should appear.

---

# 📸 Figure 12 — Load Balancer Test

<p align="center">
  <img src="./screenshots/load-balancer-test.png" width="950"/>
</p>

<p align="center">
  <em>Figure 12: Load Balancer Web Application</em>
</p>

---

# 📈 Task 5 — Test Auto Scaling

## Open CloudWatch

Go to:

```text
CloudWatch → All alarms
```

You should see:

- AlarmHigh
- AlarmLow

---

# 📸 Figure 13 — CloudWatch Alarms

<p align="center">
  <img src="./screenshots/cloudwatch-alarms.png" width="950"/>
</p>

<p align="center">
  <em>Figure 13: CloudWatch Alarms</em>
</p>

---

## Generate CPU Load

Return to the web application.

Click:

```text
Load Test
```

This generates CPU load.

---

# 📸 Figure 14 — Load Testing

<p align="center">
  <img src="./screenshots/load-test.png" width="950"/>
</p>

<p align="center">
  <em>Figure 14: Application Load Test</em>
</p>

---

## Verify Scaling

Return to:

```text
EC2 → Instances
```

Additional instances should automatically launch.

---

# 📸 Figure 15 — Auto Scaling in Action

<p align="center">
  <img src="./screenshots/scaled-instances.png" width="950"/>
</p>

<p align="center">
  <em>Figure 15: Automatically Scaled EC2 Instances</em>
</p>

---

# 🗑️ Task 6 — Terminate Web Server 1

Select:

```text
Web Server 1
```

Go to:

```text
Instance State → Terminate Instance
```

Click:

```text
Terminate
```

---

# 📸 Figure 16 — Terminate Instance

<p align="center">
  <img src="./screenshots/terminate-instance.png" width="950"/>
</p>

<p align="center">
  <em>Figure 16: Terminate Web Server 1</em>
</p>

---

# ✅ Conclusion

In this lab, you successfully:

- Created an AMI
- Created a Target Group
- Configured an Application Load Balancer
- Created a Launch Template
- Configured an Auto Scaling Group
- Automatically scaled EC2 instances
- Monitored infrastructure using CloudWatch

---

# 🧠 Key AWS Services Used

| Service | Purpose |
|---|---|
| EC2 | Virtual servers |
| ELB | Traffic distribution |
| Auto Scaling | Automatic instance scaling |
| CloudWatch | Monitoring and alarms |
| AMI | EC2 image template |

---

# 📌 Final Notes

This architecture improves:

- High availability
- Fault tolerance
- Scalability
- Performance
- Resource optimization

---

# 🏁 Lab Complete

Congratulations 🎉

You completed:

✅ AMI Creation  
✅ Load Balancer Configuration  
✅ Auto Scaling Group Deployment  
✅ CloudWatch Monitoring  
✅ Automatic Scaling Test  
✅ High Availability Architecture

---
