# 🌐 AWS Lab 3 – Introduction to Amazon EC2

---

# 📌 Lab Overview and Objectives

## 🧠 Introduction

This lab provides a practical introduction to launching, managing, resizing, and monitoring an Amazon EC2 instance.

Amazon Elastic Compute Cloud (Amazon EC2) is a web service that provides resizable compute capacity in the cloud. It is designed to make web-scale cloud computing easier for developers.

Amazon EC2 provides a simple web service interface that allows users to:

- Launch virtual servers quickly
- Configure computing resources easily
- Scale infrastructure dynamically
- Manage cloud servers efficiently

Amazon EC2 gives you complete control over your computing resources and allows you to run applications on Amazon’s highly reliable cloud infrastructure.

One of the major advantages of Amazon EC2 is its flexibility:

- You can increase or decrease resources at any time
- You only pay for the resources you use
- You can deploy applications within minutes

Amazon EC2 also helps developers build highly available and fault-tolerant applications while reducing infrastructure costs.

---

# 🧠 Architectural Diagram

<p align="center">
  <img src="./screenshots/ec2-architecture.png" width="850"/>
</p>

<p align="center">
  <em>Figure 1: Amazon EC2 Web Server Architecture</em>
</p>

---

# 🎯 Lab Objectives

After completing this lab, you will be able to:

## ✅ Launch a Web Server with Termination Protection Enabled

You will create and configure an Amazon EC2 instance running a web server while enabling termination protection to prevent accidental deletion.

---

## ✅ Monitor Your EC2 Instance

You will use monitoring tools such as:

- Status Checks
- CloudWatch Metrics
- System Logs
- Instance Screenshots

to observe the health and performance of your instance.

---

## ✅ Modify the Security Group to Allow HTTP Access

You will configure inbound security group rules to allow web traffic on port 80 (HTTP) so users can access the hosted web application.

---

## ✅ Resize Your Amazon EC2 Instance

You will:

- Change the EC2 instance type
- Increase computing resources
- Modify the EBS storage volume size

This demonstrates how AWS supports scalability.

---

## ✅ Enable and Test Stop Protection

You will enable stop protection to prevent accidental stopping of your EC2 instance and test how this protection works.

---

## ✅ Explore Amazon EC2 Limits

You will use AWS Service Quotas to explore:

- EC2 instance limits
- Running instance quotas
- Resource limitations per region

---

## ✅ Stop Your EC2 Instance

You will safely stop your EC2 instance after disabling stop protection.

---

# ⏱️ Duration

Approximate time: 35 minutes

---

# 🌐 Task 1 — Launch Your Amazon EC2 Instance

## 📌 Description

In this task, you will launch an Amazon EC2 instance with:

- Termination Protection
- Stop Protection
- User Data Script
- Apache Web Server

The EC2 instance will host a simple web application accessible from the internet.

---

# ⚙️ Step 1 — Open EC2 Console

- Open AWS Management Console
- Choose ▦ **Services**
- Select **Compute**
- Choose **EC2**

Verify the region:

- `N. Virginia (us-east-1)`

---

# ⚙️ Step 2 — Launch Instance

- Click **Launch instance**
- Select **Launch instance**

---

# ⚙️ Step 3 — Configure Name and Tags

Set the instance name:

- `Web Server`

AWS automatically creates a tag:

| Key | Value |
|------|------|
| Name | Web Server |

Tags help organize AWS resources.

---

# ⚙️ Step 4 — Choose Amazon Machine Image (AMI)

Keep the default selections:

- Amazon Linux
- Amazon Linux 2023 AMI

---

# 🧠 AMI Explanation

An Amazon Machine Image (AMI) provides:

- Operating System
- Software packages
- Storage configuration
- Launch permissions

The AMI acts as the template for the EC2 instance.

---

# ⚙️ Step 5 — Choose Instance Type

Keep default instance type:

- `t2.micro`

Specifications:

- 1 vCPU
- 1 GiB RAM

---

# 🧠 Instance Type Explanation

Amazon EC2 instance types define:

- CPU resources
- Memory capacity
- Storage performance
- Network performance

The `t2.micro` instance is ideal for lightweight workloads and AWS labs.

---

# ⚙️ Step 6 — Configure Key Pair

Select:

- `vockey`

---

# 🧠 Key Pair Explanation

AWS uses public-key cryptography for secure authentication.

The key pair allows:

- Secure SSH access
- Secure instance authentication

In this lab, the key pair is required even if SSH is not used.

---

# ⚙️ Step 7 — Configure Network Settings

Choose:

- VPC: `Lab VPC`
- Subnet: `PublicSubnet1`

Keep:

- Auto-assign Public IP enabled

---

# 🌐 Configure Security Group

Choose:

- Create security group

Configure:

| Parameter | Value |
|------|------|
| Security Group Name | Web Server security group |
| Description | Security group for my web server |

Remove the default inbound SSH rule.

---

# 🧠 Security Group Explanation

A security group acts as a virtual firewall.

It controls:

- Inbound traffic
- Outbound traffic

Currently:

- No inbound rules are configured
- HTTP access will be added later

---

# ⚙️ Step 8 — Configure Storage

Keep default storage settings:

| Volume Type | Size |
|------|------|
| gp3 SSD | 8 GiB |

---

# 🧠 Storage Explanation

Amazon EC2 uses Amazon Elastic Block Store (EBS).

The root volume:

- Stores the operating system
- Stores applications and data

---

# ⚙️ Step 9 — Configure Advanced Details

Expand:

- Advanced details

Enable:

- Termination Protection

---

# 🧠 Termination Protection

Termination protection prevents accidental deletion of the EC2 instance.

When enabled:

- The instance cannot be terminated unless protection is disabled first.

---

# ⚙️ Step 10 — Configure User Data Script

Paste the following script into the **User data** field:

```bash
#!/bin/bash

dnf install -y httpd

systemctl enable httpd

systemctl start httpd

echo '<html><h1>Hello From Your Web Server!</h1></html>' > /var/www/html/index.html
```
---


# ⚙️ Step 11 — Launch the Instance

At the bottom of the **Summary** panel:

- Click **Launch instance**

AWS will begin provisioning the EC2 instance.

You should now see:

- ✅ Success message

---

# ⚙️ Step 12 — View All Instances

After the instance is launched:

- Choose **View all instances**

In the **Instances** list:

- Select `Web Server`

Review the information displayed in the **Details** tab.

The Details section contains important information about:

- Instance type
- Security settings
- Network configuration
- Public IPv4 address
- Public IPv4 DNS
- Instance ID
- VPC and Subnet information

---

# EC2 Instance Details

<p align="center">
  <img src="./screenshots/ec2-details.png" width="750"/>
</p>

<p align="center">
  <em>Figure 2: Amazon EC2 Instance Details</em>
</p>

---

# 🧠 Public IPv4 DNS Explanation

The instance is automatically assigned a:

- Public IPv4 DNS

This DNS name allows communication with the EC2 instance directly from the Internet.

Example:

```text
ec2-44-xxx-xxx-xxx.compute-1.amazonaws.com
```

This public DNS can later be used to:

- Access web applications
- Connect using SSH
- Test hosted services

---

# ⚙️ Step 13 — Wait for Instance Initialization

At first, the EC2 instance will appear in the following states:

| State | Description |
|---|---|
| Pending | AWS is launching the instance |
| Initializing | Operating system and services are starting |
| Running | The instance is fully operational |

---

# 🧠 Instance State Explanation

### 🔹 Pending
AWS allocates:
- Virtual hardware
- Storage
- Networking resources

### 🔹 Initializing
Amazon Linux boots and:
- Executes the User Data script
- Starts Apache Web Server
- Configures services

### 🔹 Running
The EC2 instance becomes available for:
- Monitoring
- Remote access
- Web hosting

---

# Running EC2 Instance

<p align="center">
  <img src="./screenshots/ec2-running.png" width="750"/>
</p>

<p align="center">
  <em>Figure 3: EC2 Instance Running Successfully</em>
</p>

---

# ✅ Verification

Wait until the instance displays the following status:

| Parameter | Status |
|---|---|
| Instance State | Running |
| Status Checks | 2/2 checks passed |

---

# 🧠 Status Checks Explanation

Amazon EC2 automatically performs two health checks:

| Status Check | Description |
|---|---|
| System Status Check | Verifies AWS physical infrastructure |
| Instance Status Check | Verifies the operating system inside the instance |

When both checks pass:

- The AWS infrastructure is healthy
- The operating system is functioning correctly
- The instance is ready for use

---

# Status Checks Passed

<p align="center">
  <img src="./screenshots/status-checks-passed.png" width="750"/>
</p>

<p align="center">
  <em>Figure 4: EC2 Status Checks Successfully Passed</em>
</p>

---

# 🎉 Congratulations!


The instance is now:

✅ Running successfully  
✅ Connected to the network  
✅ Protected against accidental termination  
✅ Configured with Apache Web Server  
✅ Ready for monitoring and web access  

---

# ✅ Result

You successfully created and configured an Amazon EC2 instance with:

- Amazon Linux 2023
- Apache Web Server
- User Data Automation
- Public IPv4 Connectivity
- Security Group Configuration
- Termination Protection

The environment is now ready for the next tasks in the lab.

---

# 🌐 Task 2 — Monitor Your Instance

## 📌 Description

Monitoring is an important part of maintaining the:

- Reliability
- Availability
- Performance

of your Amazon EC2 instances and AWS infrastructure.

In this task, you will explore several EC2 monitoring and troubleshooting tools.

---

## ⚙️ Step 1 — Open Status Checks

Select:

- `Web Server` instance

Choose:

- **Status checks** tab

### 🧠 Status Checks Explanation

Amazon EC2 automatically performs health checks on every running instance.

There are two types of status checks:

| Status Check | Description |
|--------------|-------------|
| System Reachability | Verifies AWS infrastructure and hardware |
| Instance Reachability | Verifies the operating system and instance responsiveness |

Both checks must pass successfully.


## ✅ Expected Result

Verify:

| Parameter | Status |
|-----------|--------|
| System Reachability | Passed |
| Instance Reachability | Passed |

This confirms that:
- The AWS infrastructure is healthy
- The operating system is functioning correctly

---

## ⚙️ Step 2 — Open Monitoring Tab

Choose:

- **Monitoring** tab

### 🧠 Monitoring Explanation

The Monitoring tab displays metrics collected by Amazon CloudWatch.

Examples of metrics:
- CPU Utilization
- Network Traffic
- Disk Operations
- Status Check Metrics

Basic monitoring: updates every 5 minutes  
Detailed monitoring: updates every 1 minute

<p align="center">
  <img src="./screenshots/cloudwatch-monitoring.png" width="750"/>
</p>
<p align="center">
  <em>Figure 5: Amazon CloudWatch Monitoring Metrics</em>
</p>

---

## 📌 Expand Monitoring Graphs

To enlarge a graph:
- Click the three dots icon
- Select **Enlarge**

This provides better visibility and detailed metric analysis.

<p align="center">
  <img src="./screenshots/enlarged-monitoring.png" width="750"/>
</p>
<p align="center">
  <em>Figure 6: Enlarged CloudWatch Metric Graph</em>
</p>

---

## ⚙️ Step 3 — Open System Log

Choose:
- **Actions**
- **Monitor and troubleshoot**
- **Get system log**

### 🧠 System Log Explanation

The System Log displays:
- Console boot messages
- Kernel logs
- Service startup logs
- Error messages

This tool is extremely useful for:
- Troubleshooting boot problems
- Diagnosing service failures
- Investigating unreachable instances

---

## 📌 Verify Apache Installation

Scroll through the log output and verify:
- HTTP package installation
- Apache service startup

These actions were executed automatically by the User Data script.

```bash
[    0.481741] Cannot get hvm parameter CONSOLE_EVTCHN (18): -22!
[    7.411225] systemd[1]: Finished nfs-convert.service - Preprocess NFS configuration convertion.
[    7.431812] systemd[1]: modprobe@drm.service: Deactivated successfully.
[    7.453875] device-mapper: ioctl: 4.47.0-ioctl (2022-07-28) initialised: dm-devel@redhat.com
[    7.465326] systemd[1]: Finished modprobe@drm.service - Load Kernel Module drm.
[    7.477642] systemd[1]: Started systemd-journald.service - Journal Service.
[    7.526586] loop: module loaded
[    7.532365] fuse: init (API version 7.38)
[    7.692695] systemd-journald[1095]: Received client request to flush runtime journal.
[    8.043633] RPC: Registered named UNIX socket transport module.
[    8.050241] RPC: Registered udp transport module.
[    8.056194] RPC: Registered tcp transport module.
[    8.061351] RPC: Registered tcp NFSv4.1 backchannel transport module.
[    8.333657] vif vif-0 enX0: renamed from eth0
[    8.357718] input: Power Button as /devices/LNXSYSTM:00/LNXPWRBN:00/input/input0
[    8.458701] SCSI subsystem initialized
[    8.486923] i8042: PNP: PS/2 Controller [PNP0303:PS2K,PNP0f13:PS2M] at 0x60,0x64 irq 1,12
[    8.499392] serio: i8042 KBD port at 0x60,0x64 irq 1
[    8.504318] serio: i8042 AUX port at 0x60,0x64 irq 12
[    8.523560] ACPI: button: Power Button [PWRF]
[    8.529040] input: Sleep Button as /devices/LNXSYSTM:00/LNXSLPBN:00/input/input1
[    8.627200] ACPI: button: Sleep Button [SLPF]
[    8.786546] libata version 3.00 loaded.
[    8.820714] zram_generator::config[1898]: zram0: system has too much memory (961MB), limit is 800MB, ignoring.
[    8.832652] ata_piix 0000:00:01.1: version 2.13
[    8.904892] scsi host0: ata_piix
[    8.915017] scsi host1: ata_piix
[    8.915835] ata1: PATA max MWDMA2 cmd 0x1f0 ctl 0x3f6 bmdma 0xc100 irq 14
[    8.915837] ata2: PATA max MWDMA2 cmd 0x170 ctl 0x376 bmdma 0xc108 irq 15
[   26.795849] cloud-init[2081]: Cloud-init v. 22.2.2 running 'init' at Sat, 23 May 2026 17:01:46 +0000. Up 26.73 seconds.
[   27.644045] cloud-init[2081]: ci-info: ++++++++++++++++++++++++++++++++++++++++Net device info++++++++++++++++++++++++++++++++++++++++
[   27.654923] cloud-init[2081]: ci-info: +--------+------+------------------------------+-----------------+--------+-------------------+
[   27.665022] cloud-init[2081]: ci-info: | Device |  Up  |           Address            |       Mask      | Scope  |     Hw-Address    |
[   27.675149] cloud-init[2081]: ci-info: +--------+------+------------------------------+-----------------+--------+-------------------+
[   27.685789] cloud-init[2081]: ci-info: |  enX0  | True |          10.0.1.12           | 255.255.255.240 | global | 12:6b:4b:58:7e:5b |
[   27.695732] cloud-init[2081]: ci-info: |  enX0  | True | fe80::106b:4bff:fe58:7e5b/64 |        .        |  link  | 12:6b:4b:58:7e:5b |
[   27.706814] cloud-init[2081]: ci-info: |   lo   | True |          127.0.0.1           |    255.0.0.0    |  host  |         .         |
[   27.716813] cloud-init[2081]: ci-info: |   lo   | True |           ::1/128            |        .        |  host  |         .         |
[   27.726804] cloud-init[2081]: ci-info: +--------+------+------------------------------+-----------------+--------+-------------------+
[   27.737164] cloud-init[2081]: ci-info: ++++++++++++++++++++++++++++Route IPv4 info+++++++++++++++++++++++++++++
[   27.745399] cloud-init[2081]: ci-info: +-------+-------------+----------+-----------------+-----------+-------+
[   27.753861] cloud-init[2081]: ci-info: | Route | Destination | Gateway  |     Genmask     | Interface | Flags |
[   27.765141] cloud-init[2081]: ci-info: +-------+-------------+----------+-----------------+-----------+-------+
[   27.774694] cloud-init[2081]: ci-info: |   0   |   0.0.0.0   | 10.0.1.1 |     0.0.0.0     |    enX0   |   UG  |
[   27.782797] cloud-init[2081]: ci-info: |   1   |   10.0.0.2  | 10.0.1.1 | 255.255.255.255 |    enX0   |  UGH  |
[   27.793274] cloud-init[2081]: ci-info: |   2   |   10.0.1.0  | 0.0.0.0  | 255.255.255.240 |    enX0   |   U   |
[   27.803505] cloud-init[2081]: ci-info: |   3   |   10.0.1.1  | 0.0.0.0  | 255.255.255.255 |    enX0   |   UH  |
[   27.812846] cloud-init[2081]: ci-info: +-------+-------------+----------+-----------------+-----------+-------+
[   27.819739] cloud-init[2081]: ci-info: +++++++++++++++++++Route IPv6 info+++++++++++++++++++
[   27.827257] cloud-init[2081]: ci-info: +-------+-------------+---------+-----------+-------+
[   27.834924] cloud-init[2081]: ci-info: | Route | Destination | Gateway | Interface | Flags |
[   27.840720] cloud-init[2081]: ci-info: +-------+-------------+---------+-----------+-------+
[   27.849012] cloud-init[2081]: ci-info: |   0   |  fe80::/64  |    ::   |    enX0   |   U   |
[   27.856033] cloud-init[2081]: ci-info: |   2   |    local    |    ::   |    enX0   |   U   |
[   27.864550] cloud-init[2081]: ci-info: |   3   |  multicast  |    ::   |    enX0   |   U   |
[   27.871098] cloud-init[2081]: ci-info: +-------+-------------+---------+-----------+-------+
[   30.678224] cloud-init[2081]: Generating public/private ed25519 key pair.
[   30.690278] cloud-init[2081]: Your identification has been saved in /etc/ssh/ssh_host_ed25519_key
[   30.710113] cloud-init[2081]: Your public key has been saved in /etc/ssh/ssh_host_ed25519_key.pub
[   30.716448] cloud-init[2081]: The key fingerprint is:
[   30.728640] cloud-init[2081]: SHA256:T0TME5y+H1Agm76o1FY51oJ1cztX0Wkf5kE7OL2Z+Vs root@ip-10-0-1-12.ec2.internal
[   30.750173] cloud-init[2081]: The key's randomart image is:
[   30.754891] cloud-init[2081]: +--[ED25519 256]--+
[   30.757959] cloud-init[2081]: |        .+++  .oo|
[   30.763071] cloud-init[2081]: |         =* . o*+|
[   30.766209] cloud-init[2081]: |        +.+o.o++=|
[   30.769595] cloud-init[2081]: |       + =oo ..oB|
[   30.772880] cloud-init[2081]: |      . S ooo .= |
[   30.777532] cloud-init[2081]: |     . + *. .o  .|
[   30.782138] cloud-init[2081]: |    . + . .. .  E|
[   30.786655] cloud-init[2081]: |   . o      .   o|
[   30.795374] cloud-init[2081]: |    .          . |
[   30.800163] cloud-init[2081]: +----[SHA256]-----+
[   30.804593] cloud-init[2081]: Generating public/private ecdsa key pair.
[   30.830248] cloud-init[2081]: Your identification has been saved in /etc/ssh/ssh_host_ecdsa_key
[   30.836851] cloud-init[2081]: Your public key has been saved in /etc/ssh/ssh_host_ecdsa_key.pub
[   30.856495] cloud-init[2081]: The key fingerprint is:
[   30.881857] cloud-init[2081]: SHA256:RF9u8XfFkMzSulYnlFVLVZod2YWD0hDEOOV7CSG6yEY root@ip-10-0-1-12.ec2.internal
[   30.926169] cloud-init[2081]: The key's randomart image is:
[   30.956704] cloud-init[2081]: +---[ECDSA 256]---+
[   30.969346] cloud-init[2081]: |        o=*+o=.X#|
[   30.985457] cloud-init[2081]: |       oo+o++o@=*|
[   30.998675] cloud-init[2081]: |    E . ..+.o++o+|
[   31.010188] cloud-init[2081]: |   o . o   +..o.o|
[   31.030125] cloud-init[2081]: |    + . S . oo o |
[   31.039735] cloud-init[2081]: |   .       .o    |
[   31.062751] cloud-init[2081]: |           .     |
[   31.072112] cloud-init[2081]: |                 |
[   31.079851] cloud-init[2081]: |                 |
[   31.087495] cloud-init[2081]: +----[SHA256]-----+
2026/05/23 17:01:51Z: SSM Agent unable to acquire credentials: <error>no valid credentials could be retrieved for ec2 identity. Default Host Management Err: error calling RequestManagedInstanceRoleToken: AccessDeniedException: Systems Manager's instance management role is not configured for account: 891377141696
	status code: 400, request id: f0f16c94-b70f-4ac1-9231-0656143c36d3</error>
[   31.647351] cloud-init[2171]: Cloud-init v. 22.2.2 running 'modules:config' at Sat, 23 May 2026 17:01:51 +0000. Up 31.55 seconds.
[   33.399770] cloud-init[2179]: Cloud-init v. 22.2.2 running 'modules:final' at Sat, 23 May 2026 17:01:53 +0000. Up 33.34 seconds.
[   37.061390] cloud-init[2179]: Amazon Linux 2023 repository                     31 MB/s |  64 MB     00:02


Amazon Linux 2023.11.20260514
Kernel 6.1.170-213.321.amzn2023.x86_64 on an x86_64 (-)

ip-10-0-1-12 login: [   53.501291] cloud-init[2179]: Amazon Linux 2023 Kernel Livepatch repository   104 kB/s |  38 kB     00:00
[   54.900388] cloud-init[2179]: Dependencies resolved.
[   54.960125] cloud-init[2179]: ================================================================================
[   54.967599] cloud-init[2179]:  Package               Arch     Version                     Repository     Size
[   54.978181] cloud-init[2179]: ================================================================================
[   54.990098] cloud-init[2179]: Installing:
[   54.993183] cloud-init[2179]:  httpd                 x86_64   2.4.66-1.amzn2023.0.1       amazonlinux    47 k
[   54.999587] cloud-init[2179]: Installing dependencies:
[   55.003664] cloud-init[2179]:  apr                   x86_64   1.7.5-1.amzn2023.0.4        amazonlinux   129 k
[   55.020101] cloud-init[2179]:  apr-util              x86_64   1.6.3-1.amzn2023.0.2        amazonlinux    97 k
[   55.027272] cloud-init[2179]:  apr-util-lmdb         x86_64   1.6.3-1.amzn2023.0.2        amazonlinux    13 k
[   55.035464] cloud-init[2179]:  generic-logos-httpd   noarch   18.0.0-12.amzn2023.0.3      amazonlinux    19 k
[   55.041727] cloud-init[2179]:  httpd-core            x86_64   2.4.66-1.amzn2023.0.1       amazonlinux   1.4 M
[   55.047924] cloud-init[2179]:  httpd-filesystem      noarch   2.4.66-1.amzn2023.0.1       amazonlinux    13 k
[   55.054908] cloud-init[2179]:  httpd-tools           x86_64   2.4.66-1.amzn2023.0.1       amazonlinux    81 k
[   55.064239] cloud-init[2179]:  libbrotli             x86_64   1.0.9-4.amzn2023.0.2        amazonlinux   315 k
[   55.076152] cloud-init[2179]:  mailcap               noarch   2.1.49-3.amzn2023.0.3       amazonlinux    33 k
[   55.083408] cloud-init[2179]: Installing weak dependencies:
[   55.090090] cloud-init[2179]:  apr-util-openssl      x86_64   1.6.3-1.amzn2023.0.2        amazonlinux    15 k
[   55.097323] cloud-init[2179]:  mod_http2             x86_64   2.0.27-1.amzn2023.0.3       amazonlinux   166 k
[   55.115263] cloud-init[2179]:  mod_lua               x86_64   2.4.66-1.amzn2023.0.1       amazonlinux    60 k
[   55.123571] cloud-init[2179]: Transaction Summary
[   55.127841] cloud-init[2179]: ================================================================================
[   55.136501] cloud-init[2179]: Install  13 Packages
[   55.140244] cloud-init[2179]: Total download size: 2.4 M
[   55.145286] cloud-init[2179]: Installed size: 6.9 M
[   55.149917] cloud-init[2179]: Downloading Packages:
[   55.156779] cloud-init[2179]: (1/13): apr-util-lmdb-1.6.3-1.amzn2023.0.2.x86_ 168 kB/s |  13 kB     00:00
[   55.165843] cloud-init[2179]: (2/13): apr-1.7.5-1.amzn2023.0.4.x86_64.rpm     1.4 MB/s | 129 kB     00:00
[   55.173706] cloud-init[2179]: (3/13): apr-util-1.6.3-1.amzn2023.0.2.x86_64.rp 1.0 MB/s |  97 kB     00:00
[   55.182003] cloud-init[2179]: (4/13): apr-util-openssl-1.6.3-1.amzn2023.0.2.x 601 kB/s |  15 kB     00:00
[   55.199820] cloud-init[2179]: (5/13): httpd-2.4.66-1.amzn2023.0.1.x86_64.rpm  1.8 MB/s |  47 kB     00:00
[   55.219896] cloud-init[2179]: (6/13): generic-logos-httpd-18.0.0-12.amzn2023. 510 kB/s |  19 kB     00:00
[   55.229802] cloud-init[2179]: (7/13): httpd-core-2.4.66-1.amzn2023.0.1.x86_64  29 MB/s | 1.4 MB     00:00
[   55.238446] cloud-init[2179]: (8/13): httpd-filesystem-2.4.66-1.amzn2023.0.1. 343 kB/s |  13 kB     00:00
[   55.249016] cloud-init[2179]: (9/13): httpd-tools-2.4.66-1.amzn2023.0.1.x86_6 2.1 MB/s |  81 kB     00:00
[   55.265879] cloud-init[2179]: (10/13): libbrotli-1.0.9-4.amzn2023.0.2.x86_64. 8.7 MB/s | 315 kB     00:00
[   55.274020] cloud-init[2179]: (11/13): mailcap-2.1.49-3.amzn2023.0.3.noarch.r 955 kB/s |  33 kB     00:00
[   55.282366] cloud-init[2179]: (12/13): mod_http2-2.0.27-1.amzn2023.0.3.x86_64 4.9 MB/s | 166 kB     00:00
[   55.296320] cloud-init[2179]: (13/13): mod_lua-2.4.66-1.amzn2023.0.1.x86_64.r 2.3 MB/s |  60 kB     00:00
[   55.306283] cloud-init[2179]: --------------------------------------------------------------------------------
[   55.320123] cloud-init[2179]: Total                                           7.4 MB/s | 2.4 MB     00:00
[   55.520148] cloud-init[2179]: Running transaction check
[   55.559963] cloud-init[2179]: Transaction check succeeded.
[   55.580160] cloud-init[2179]: Running transaction test
[   55.807738] cloud-init[2179]: Transaction test succeeded.
[   55.816256] cloud-init[2179]: Running transaction
[   56.019440] cloud-init[2179]:   Preparing        :                                                        1/1
[   56.042724] cloud-init[2179]:   Installing       : apr-1.7.5-1.amzn2023.0.4.x86_64                       1/13
[   56.059059] cloud-init[2179]:   Installing       : apr-util-lmdb-1.6.3-1.amzn2023.0.2.x86_64             2/13
[   56.074065] cloud-init[2179]:   Installing       : apr-util-openssl-1.6.3-1.amzn2023.0.2.x86_64          3/13
[   56.092795] cloud-init[2179]:   Installing       : apr-util-1.6.3-1.amzn2023.0.2.x86_64                  4/13
[   56.121241] cloud-init[2179]:   Installing       : mailcap-2.1.49-3.amzn2023.0.3.noarch                  5/13
[   56.148725] cloud-init[2179]:   Installing       : httpd-tools-2.4.66-1.amzn2023.0.1.x86_64              6/13
[   56.168734] cloud-init[2179]:   Installing       : libbrotli-1.0.9-4.amzn2023.0.2.x86_64                 7/13
[   56.288580] cloud-init[2179]:   Running scriptlet: httpd-filesystem-2.4.66-1.amzn2023.0.1.noarch         8/13
[   56.645757] cloud-init[2179]:   Installing       : httpd-filesystem-2.4.66-1.amzn2023.0.1.noarch         8/13
[   56.681500] cloud-init[2179]:   Installing       : httpd-core-2.4.66-1.amzn2023.0.1.x86_64               9/13
[   56.697694] cloud-init[2179]:   Installing       : mod_http2-2.0.27-1.amzn2023.0.3.x86_64               10/13
[   56.709576] cloud-init[2179]:   Installing       : mod_lua-2.4.66-1.amzn2023.0.1.x86_64                 11/13
[   56.731219] cloud-init[2179]:   Installing       : generic-logos-httpd-18.0.0-12.amzn2023.0.3.noarch    12/13
[   56.747752] cloud-init[2179]:   Installing       : httpd-2.4.66-1.amzn2023.0.1.x86_64                   13/13
[   57.942340] zram_generator::config[2443]: zram0: system has too much memory (961MB), limit is 800MB, ignoring.
[   57.981119] cloud-init[2179]:   Running scriptlet: httpd-2.4.66-1.amzn2023.0.1.x86_64                   13/13
[   57.998010] cloud-init[2179]:   Verifying        : apr-1.7.5-1.amzn2023.0.4.x86_64                       1/13
[   58.010224] cloud-init[2179]:   Verifying        : apr-util-1.6.3-1.amzn2023.0.2.x86_64                  2/13
[   58.021027] cloud-init[2179]:   Verifying        : apr-util-lmdb-1.6.3-1.amzn2023.0.2.x86_64             3/13
[   58.031258] cloud-init[2179]:   Verifying        : apr-util-openssl-1.6.3-1.amzn2023.0.2.x86_64          4/13
[   58.040541] cloud-init[2179]:   Verifying        : generic-logos-httpd-18.0.0-12.amzn2023.0.3.noarch     5/13
[   58.049452] cloud-init[2179]:   Verifying        : httpd-2.4.66-1.amzn2023.0.1.x86_64                    6/13
[   58.058312] cloud-init[2179]:   Verifying        : httpd-core-2.4.66-1.amzn2023.0.1.x86_64               7/13
[   58.068317] cloud-init[2179]:   Verifying        : httpd-filesystem-2.4.66-1.amzn2023.0.1.noarch         8/13
[   58.076919] cloud-init[2179]:   Verifying        : httpd-tools-2.4.66-1.amzn2023.0.1.x86_64              9/13
[   58.088127] cloud-init[2179]:   Verifying        : libbrotli-1.0.9-4.amzn2023.0.2.x86_64                10/13
[   58.098937] cloud-init[2179]:   Verifying        : mailcap-2.1.49-3.amzn2023.0.3.noarch                 11/13
[   58.108134] cloud-init[2179]:   Verifying        : mod_http2-2.0.27-1.amzn2023.0.3.x86_64               12/13
[   58.617624] cloud-init[2179]:   Verifying        : mod_lua-2.4.66-1.amzn2023.0.1.x86_64                 13/13
[   58.632170] cloud-init[2179]: Installed:
[   58.637990] cloud-init[2179]:   apr-1.7.5-1.amzn2023.0.4.x86_64
[   58.647500] cloud-init[2179]:   apr-util-1.6.3-1.amzn2023.0.2.x86_64
[   58.653453] cloud-init[2179]:   apr-util-lmdb-1.6.3-1.amzn2023.0.2.x86_64
[   58.663858] cloud-init[2179]:   apr-util-openssl-1.6.3-1.amzn2023.0.2.x86_64
[   58.675261] cloud-init[2179]:   generic-logos-httpd-18.0.0-12.amzn2023.0.3.noarch
[   58.687330] cloud-init[2179]:   httpd-2.4.66-1.amzn2023.0.1.x86_64
[   58.696533] cloud-init[2179]:   httpd-core-2.4.66-1.amzn2023.0.1.x86_64
[   58.703999] cloud-init[2179]:   httpd-filesystem-2.4.66-1.amzn2023.0.1.noarch
[   58.711960] cloud-init[2179]:   httpd-tools-2.4.66-1.amzn2023.0.1.x86_64
[   58.722372] cloud-init[2179]:   libbrotli-1.0.9-4.amzn2023.0.2.x86_64
[   58.729939] cloud-init[2179]:   mailcap-2.1.49-3.amzn2023.0.3.noarch
[   58.735113] cloud-init[2179]:   mod_http2-2.0.27-1.amzn2023.0.3.x86_64
[   58.750290] cloud-init[2179]:   mod_lua-2.4.66-1.amzn2023.0.1.x86_64
[   58.770178] cloud-init[2179]: Complete!
[   58.844307] cloud-init[2179]: Created symlink /etc/systemd/system/multi-user.target.wants/httpd.service â /usr/lib/systemd/system/httpd.service.
[   59.426384] zram_generator::config[3687]: zram0: system has too much memory (961MB), limit is 800MB, ignoring.
ci-info: +++++++++++++++++++++++++++Authorized keys from /home/ec2-user/.ssh/authorized_keys for user ec2-user++++++++++++++++++++++++++++
ci-info: +---------+-------------------------------------------------------------------------------------------------+---------+---------+
ci-info: | Keytype |                                       Fingerprint (sha256)                                      | Options | Comment |
ci-info: +---------+-------------------------------------------------------------------------------------------------+---------+---------+
ci-info: | ssh-rsa | b4:be:54:1d:05:ef:98:4d:72:c5:11:b5:c2:d1:a0:88:c7:8d:46:d7:80:31:6d:14:0f:26:aa:0d:87:d4:bc:e4 |    -    |  vockey |
ci-info: +---------+-------------------------------------------------------------------------------------------------+---------+---------+
<14>May 23 17:02:19 cloud-init: #############################################################
<14>May 23 17:02:19 cloud-init: -----BEGIN SSH HOST KEY FINGERPRINTS-----
<14>May 23 17:02:19 cloud-init: 256 SHA256:RF9u8XfFkMzSulYnlFVLVZod2YWD0hDEOOV7CSG6yEY root@ip-10-0-1-12.ec2.internal (ECDSA)
<14>May 23 17:02:19 cloud-init: 256 SHA256:T0TME5y+H1Agm76o1FY51oJ1cztX0Wkf5kE7OL2Z+Vs root@ip-10-0-1-12.ec2.internal (ED25519)
<14>May 23 17:02:19 cloud-init: -----END SSH HOST KEY FINGERPRINTS-----
<14>May 23 17:02:19 cloud-init: #############################################################
-----BEGIN SSH HOST KEY KEYS-----
ecdsa-sha2-nistp256 AAAAE2VjZHNhLXNoYTItbmlzdHAyNTYAAAAIbmlzdHAyNTYAAABBBDYmTR4grXfB+MXhmk28E2nNpgeQmcToeEzO5FXZoqNfaR1M1bAiEmvclb7nKXeStXk0ZMXinYkHuHb2O+Cce2E= root@ip-10-0-1-12.ec2.internal
ssh-ed25519 AAAAC3NzaC1lZDI1NTE5AAAAIPFHWBno1kIAOplOgFbKFL7IzD2DovT5RCj/iM7IRNOG root@ip-10-0-1-12.ec2.internal
-----END SSH HOST KEY KEYS-----
[   59.696501] cloud-init[2179]: Cloud-init v. 22.2.2 finished at Sat, 23 May 2026 17:02:19 +0000. Datasource DataSourceEc2.  Up 59.68 seconds

```
---

## ⚙️ Step 4 — Get Instance Screenshot

Choose:
- **Actions**
- **Monitor and troubleshoot**
- **Get instance screenshot**

### 🧠 Instance Screenshot Explanation

The instance screenshot displays the console screen of the virtual machine.

This feature is useful when:
- SSH access fails
- RDP access fails
- The operating system crashes
- The server becomes unresponsive

It helps administrators visually diagnose problems.

<p align="center">
  <img src="./screenshots/instance-screenshot.png" width="750"/>
</p>
<p align="center">
  <em>Figure 5: Amazon EC2 Console Screenshot</em>
</p>

---

## 🧠 Screenshot Analysis

The screenshot shows:
- Amazon Linux 2023 boot screen
- Linux kernel version
- System service messages
- Console login prompt

This confirms that:
- The operating system booted successfully
- The EC2 instance is operational

---

## ✅ Result

You successfully explored multiple EC2 monitoring tools:

- Status Checks
- CloudWatch Metrics
- System Logs
- Instance Screenshot

These tools are essential for:
- Troubleshooting
- Monitoring performance
- Diagnosing failures
- Maintaining AWS infrastructure

---

## 🎓 Conclusion

Amazon EC2 monitoring capabilities provide administrators with powerful tools to:
- Observe instance health
- Detect failures
- Analyze performance
- Troubleshoot operating system problems

Monitoring is a critical component of cloud infrastructure management and cybersecurity operations.

---
---
# 🌐 Task 3 — Update Your Security Group and Access the Web Server

## 📌 Description

When you launched the EC2 instance, you configured a User Data script that:

- Installed Apache Web Server
- Started the HTTP service
- Created a simple web page

In this task, you will:

- Access the EC2 web server
- Configure the Security Group
- Allow HTTP traffic on port 80

---

# ⚙️ Step 1 — Open EC2 Instance Details

Ensure the instance:

- `Web Server`

is still selected.

Choose:

- **Details** tab

---

# EC2 Instance Details

<p align="center">
  <img src="./screenshots/ec2-details.png" width="850"/>
</p>

<p align="center">
  <em>Figure 1: Amazon EC2 Instance Details</em>
</p>

---

# ⚙️ Step 2 — Copy Public IPv4 Address

Copy:

- Public IPv4 address

Example:

```text
44.xxx.xxx.xxx
```

The public IPv4 address allows internet communication with the EC2 instance.

---

# Public IPv4 Address

<p align="center">
  <img src="./screenshots/public-ip-address.png" width="850"/>
</p>

<p align="center">
  <em>Figure 2: EC2 Public IPv4 Address</em>
</p>

---

# ⚙️ Step 3 — Test Web Server Access

Open:

- A new browser tab

Paste:

- The copied Public IPv4 address

Press:

- **Enter**

---

# ❓ Question

Are you able to access your web server?

---

# ❌ Result

No, the web server is not accessible yet.

---

# 🧠 Why?

The Security Group currently blocks:

- Inbound HTTP traffic on port `80`

Port `80` is used for:

- HTTP web requests

Since no inbound HTTP rule exists, the Security Group denies incoming browser requests.

This demonstrates how AWS Security Groups function as virtual firewalls.

---

# Access Denied by Security Group

<p align="center">
  <img src="./screenshots/access-denied.png" width="850"/>
</p>

<p align="center">
  <em>Figure 3: HTTP Access Blocked by the Security Group</em>
</p>

---

# 🧠 Security Group Firewall Explanation

AWS Security Groups help protect EC2 instances by controlling:

- Allowed inbound traffic
- Allowed outbound traffic

Currently:

| Traffic Type | Status |
|---|---|
| HTTP (Port 80) | Blocked |
| HTTPS (Port 443) | Blocked |
| SSH (Port 22) | Removed |

Because HTTP traffic is blocked:

- The Apache web server cannot be reached from the internet

---

# ⚙️ Step 4 — Open Security Groups

Keep the browser tab open and return to:

- EC2 Console

In the left navigation pane choose:

- **Security Groups**

---

# Security Groups Menu

<p align="center">
  <img src="./screenshots/security-groups-menu.png" width="850"/>
</p>

<p align="center">
  <em>Figure 4: EC2 Security Groups Menu</em>
</p>

---

# ⚙️ Step 5 — Select Web Server Security Group

Select:

- `Web Server security group`

Choose:

- **Inbound rules** tab

Notice:

- No inbound rules currently exist

---

# Empty Inbound Rules

<p align="center">
  <img src="./screenshots/security-group-empty.png" width="850"/>
</p>

<p align="center">
  <em>Figure 5: Empty Inbound Security Group Rules</em>
</p>

---

# ⚙️ Step 6 — Add HTTP Rule

Choose:

- **Edit inbound rules**

Then:

- Select **Add rule**

Configure the following:

| Parameter | Value |
|---|---|
| Type | HTTP |
| Protocol | TCP |
| Port Range | 80 |
| Source | Anywhere-IPv4 |

Choose:

- **Save rules**

---

# HTTP Inbound Rule Configuration

<p align="center">
  <img src="./screenshots/http-rule.png" width="850"/>
</p>

<p align="center">
  <em>Figure 6: Configuring HTTP Security Group Rule</em>
</p>

---

# 🧠 HTTP Rule Explanation

This rule allows:

- Incoming web traffic on port 80

Source:

```text
0.0.0.0/0
```

Meaning:

- Any IPv4 address on the internet can access the web server

This configuration makes the Apache website publicly accessible.

---

# 🧠 Security Group Analysis

The Security Group now contains the following inbound rule:

| Type | Protocol | Port | Source |
|---|---|---|---|
| HTTP | TCP | 80 | 0.0.0.0/0 |

---

# Security Group Rule Analysis

<p align="center">
  <img src="./screenshots/security-group-analysis.png" width="850"/>
</p>

<p align="center">
  <em>Figure 7: Security Group Allowing HTTP Traffic</em>
</p>

---

# ⚙️ Step 7 — Refresh the Web Browser

Return to:

- The web browser tab previously opened

Refresh the page.

---

# ✅ Web Server Accessible

You should now see the following message:

```html
Hello From Your Web Server!
```

This confirms that:

- Apache Web Server is running successfully
- HTTP traffic is allowed through the Security Group
- The EC2 instance is accessible from the internet

---

# Apache Web Server Output

<p align="center">
  <img src="./screenshots/web-server-output.png" width="850"/>
</p>

<p align="center">
  <em>Figure 8: Apache Web Server Successfully Accessible</em>
</p>

---

# 🌐 HTTP Connectivity Flow

The web request now follows this path:

```text
User Browser
      ↓
Internet
      ↓
AWS Security Group
      ↓
HTTP Port 80 Allowed
      ↓
Amazon EC2 Instance
      ↓
Apache Web Server
      ↓
Web Page Response
```

---

# 📌 Important Security Note

Allowing:

```text
0.0.0.0/0
```

provides public internet access.

In production environments, administrators should:

- Restrict unnecessary access
- Use HTTPS instead of HTTP
- Configure AWS WAF protections
- Monitor traffic using CloudWatch and VPC Flow Logs

---

# ✅ Result

You successfully:

✅ Modified the Security Group  
✅ Allowed inbound HTTP traffic on port 80  
✅ Accessed the EC2 web server  
✅ Verified Apache Web Server functionality  
✅ Published a web application on AWS  

---

# 🎓 Conclusion

This task demonstrated how AWS Security Groups function as virtual firewalls to protect EC2 instances.

You learned how to:

- Control inbound traffic
- Allow HTTP access
- Publish a web application securely
- Verify internet connectivity to EC2 instances

Security Groups are one of the most important security mechanisms in AWS cloud environments.

---
