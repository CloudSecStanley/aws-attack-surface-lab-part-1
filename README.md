# AWS Attack Surface Lab – Part 1: Initial Access & Misconfigurations

## 📌 Overview

This project simulates a vulnerable AWS environment to demonstrate how attackers gain initial access through common cloud misconfigurations and how their actions can be detected using AWS CloudTrail.

The lab focuses on **real-world attack entry points**, including exposed credentials, public storage, and network misconfigurations.

---

## 🚨 Scenario

An AWS environment was intentionally configured with multiple security weaknesses:

* An over-permissive IAM user (`AdministratorAccess`)
* A publicly accessible S3 bucket
* An EC2 instance exposed to the internet via open SSH (port 22)

An attacker obtains leaked credentials and uses them to enumerate resources, access data, and explore the environment.

---

## 🎯 Objectives

* Identify common AWS attack surfaces
* Simulate attacker behavior using AWS CLI
* Analyze activity using CloudTrail logs
* Apply remediation to secure the environment

---

## 🔍 Attack Surfaces

| Type        | Resource     | Risk                                        |
| ----------- | ------------ | ------------------------------------------- |
| 🔑 Identity | IAM User     | Excessive permissions (AdministratorAccess) |
| 📦 Data     | S3 Bucket    | Public access leading to data exposure      |
| 🌐 Network  | EC2 Instance | Open SSH access to the internet             |

---

## 🧑‍💻 Attack Simulation

After obtaining leaked credentials, the attacker performed the following actions:

### 1. Identity Validation

```bash
aws sts get-caller-identity
```

### 2. Enumeration

```bash
aws iam list-users
aws s3 ls
aws ec2 describe-instances
```

### 3. Data Access

```bash
aws s3 cp s3://cloudsec-stanley-lab/test.txt .
```

These steps demonstrate typical **post-compromise reconnaissance and data access behavior**.

---

## 📊 Detection with CloudTrail

CloudTrail was used to analyze attacker activity through API logs.

### Key Observed Events:

* `GetCallerIdentity`
* `ListUsers`
* `ListBuckets`
* `DescribeInstances`

### Indicators of Compromise:

* Use of AWS CLI (`userAgent: aws-cli`)
* Enumeration of multiple services
* Access to storage resources

### Key Fields Analyzed:

* UserIdentity
* SourceIPAddress
* EventTime
* UserAgent

---

## 🔐 Remediation 

The following actions were taken to secure the environment:

* Removed `AdministratorAccess` from IAM user
* Enabled S3 Block Public Access
* Restricted SSH access to trusted IP addresses
* Reviewed CloudTrail logging configuration

---

## 📂 Findings

Detailed security findings and analysis are available here:

👉 [View Findings Report](./findings.md)

---

## 🧠 Lessons Learned

* Misconfigurations are the primary attack surface in cloud environments
* Leaked credentials can lead to full account compromise
* Enumeration is a critical phase after initial access
* CloudTrail provides visibility into attacker behavior
* Least privilege significantly reduces risk

---

## 🔗 Medium Article

Read the full breakdown of this project:

👉 https://thep3arl.medium.com/aws-attack-surface-exploitation-part-1-initial-access-c9c2758517eb

---

## 🔜 Next

Part 2 will focus on:

* IAM privilege escalation
* Persistence techniques
* Advanced detection and response

---

## 📬 Connect

* LinkedIn: https://linkedin.com/in/stanley-ndege-998822311
* Medium: https://medium.com/@thep3arl
