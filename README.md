# aws-attack-surface-lab-part-1
AWS Attack Surface Lab – Initial Access, Misconfigurations &amp; CloudTrail Detection


# AWS Attack Surface Lab – Part 1: Initial Access

## 📌 Overview

This project simulates a cloud environment with multiple security misconfigurations to demonstrate how attackers gain initial access and how their actions can be detected using AWS CloudTrail.

---

## 🚨 Scenario

The environment contains:

* Over-permissive IAM user (`AdministratorAccess`)
* Publicly accessible S3 bucket
* EC2 instance with SSH exposed to the internet

An attacker obtains leaked credentials and performs enumeration and data access activities.

---

## 🔍 Attack Surfaces

* IAM Misconfiguration (Excessive permissions)
* S3 Public Access (Data exposure)
* EC2 Network Exposure (Open SSH)

---

## 🧑‍💻 Attack Simulation

The attacker:

1. Identified identity:

```bash
aws sts get-caller-identity
```

2. Enumerated resources:

```bash
aws iam list-users
aws s3 ls
aws ec2 describe-instances
```

3. Accessed S3 data:

```bash
aws s3 cp s3://misconfigurations-lab1/test.txt .
```

---

## 📊 Detection with CloudTrail

CloudTrail logs were used to identify:

* Identity validation (`GetCallerIdentity`)
* Resource enumeration (`ListUsers`, `DescribeInstances`, `ListBuckets`)
* Data access attempts (`GetObject`)

Key fields analyzed:

* UserIdentity
* SourceIPAddress
* EventTime
* UserAgent (`aws-cli`)

---

## 🔐 Remediation

* Removed `AdministratorAccess` from IAM user
* Enabled S3 Block Public Access
* Restricted SSH access to specific IP

---

## 📂 Findings

See [findings.md](./findings.md) for detailed security analysis.

---

## 🧠 Lessons Learned

* Misconfigurations are the primary cloud attack surface
* Leaked credentials can lead to full account compromise
* CloudTrail is critical for detecting post-compromise activity
* Least privilege significantly reduces risk

---

## 🔗 Medium Article

Read the full walkthrough:
👉 https://thep3arl.medium.com/aws-attack-surface-exploitation-part-1-initial-access-c9c2758517eb
