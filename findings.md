# Security Findings – AWS Attack Surface Lab (Part 1)

## 📌 Overview

This report documents the security findings identified in an AWS environment intentionally configured with multiple misconfigurations.
The objective was to simulate attacker behavior, analyze resulting activity using CloudTrail, and identify security weaknesses.

---

## 🔓 Finding 1: Over-Permissive IAM User

**Resource:** `insecure-user`
**Issue:** The IAM user was assigned the `AdministratorAccess` policy.

### 🔍 Description

The compromised IAM user had full administrative privileges across the AWS account. This allowed unrestricted access to resources and services.

### ⚠️ Risk

* Full account compromise
* Ability to enumerate, modify, and delete resources
* Potential to disable logging and evade detection

### 🧾 Evidence

* CLI enumeration commands executed successfully
* CloudTrail logs showing API calls:

  * `ListUsers`
  * `DescribeInstances`
  * `ListBuckets`

### 🔥 Severity

**Critical**

### 🔐 Remediation

* Remove `AdministratorAccess` from the user
* Apply least privilege access policies
* Rotate compromised credentials
* Enforce IAM best practices (MFA, role-based access)

---

## 📦 Finding 2: Publicly Accessible S3 Bucket

**Resource:** `cloudsec-stanley-lab`
**Issue:** The S3 bucket allowed public read access.

### 🔍 Description

The bucket was configured with a policy that permitted unauthenticated users to access stored objects.

### ⚠️ Risk

* Data exposure to the internet
* Leakage of sensitive files or credentials
* Potential data exfiltration

### 🧾 Evidence

* Successful object retrieval via AWS CLI:

  * `aws s3 cp s3://cloudsec-stanley-lab/test.txt .`
* Bucket policy allowed public read access

### 🔥 Severity

**High**

### 🔐 Remediation

* Enable S3 Block Public Access
* Remove public bucket policies
* Restrict access using IAM policies

---

## 🌐 Finding 3: EC2 Instance Exposed via Open SSH

**Resource:** EC2 Security Group
**Issue:** SSH (port 22) was open to `0.0.0.0/0`.

### 🔍 Description

The EC2 instance was publicly accessible over SSH from any IP address, increasing exposure to unauthorized access attempts.

### ⚠️ Risk

* Brute-force login attempts
* Unauthorized access if credentials are weak or exposed
* Increased attack surface

### 🧾 Evidence

* Security group rule allowing:

  * Port 22 → `0.0.0.0/0`

### 🔥 Severity

**High**

### 🔐 Remediation

* Restrict SSH access to trusted IP addresses
* Use bastion hosts or session-based access
* Remove unnecessary inbound rules

---

## 📊 Finding 4: Limited Visibility of S3 Object-Level Access

**Resource:** CloudTrail Configuration
**Issue:** S3 object-level (data events) logging may not be fully enabled.

### 🔍 Description

While CloudTrail captured management events (IAM, EC2, S3 API calls), object-level access such as `GetObject` may not be fully visible without enabling data events.

### ⚠️ Risk

* Data access and exfiltration may go undetected
* Reduced visibility into attacker activity

### 🧾 Evidence

* CloudTrail logs showed:

  * `ListBuckets`
  * `DescribeInstances`
* Object-level visibility dependent on configuration

### 🔥 Severity

**Medium**

### 🔐 Remediation

* Enable S3 data events in CloudTrail
* Monitor sensitive buckets
* Protect log storage with restricted IAM access

---

## 🧠 Summary

This lab demonstrates how multiple misconfigurations can combine to create a high-risk cloud environment:

* Leaked credentials + excessive permissions → full account access
* Public storage → data exposure
* Network exposure → increased attack surface
* Incomplete logging → reduced detection capability

---

## 🏁 Conclusion

Cloud security incidents are often not caused by complex exploits, but by simple misconfigurations and poor access control.

Implementing least privilege, securing storage, restricting network access, and enabling comprehensive logging are critical steps in reducing cloud attack surfaces.

