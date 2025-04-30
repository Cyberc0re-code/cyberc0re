---
title: 'Raptor - Cloud Native Application Protection Platform'
published: 2023-12-22
description: 'Cloud Security Monitoring Tool'
image: './Cloud.png'
tags: [Cloud Security, Tool, Monitoring Tool]
category: 'Cloud'
draft: true
---

# Raptor – Cloud Native Application Protection Platform (CNAPP)

### [📄 View Full Project Report (PDF)](https://drive.google.com/file/d/17A0Spy1OBobnHelzcbWOctZBXWSMURxX/view?usp=sharing)

Note: This is a part of my college project.
## 🛡️ Overview

**Raptor** is a lightweight, developer-centric Cloud Native Application Protection Platform designed to bring security directly into CI/CD workflows. It focuses on automating key cloud security tasks like vulnerability scanning, policy validation, runtime threat detection, and compliance checks—without requiring heavy resources or enterprise-grade tools.

The platform was built using a mix of open-source tooling and custom automation, targeting AWS as the primary cloud environment.

---

## 🔧 Toolchain & Stack

- **AWS CLI** – Automates cloud resource interactions.
- **Prowler** – Performs compliance and misconfiguration checks against CIS benchmarks.
- **Wiz open-cvdb** – Provides live vulnerability data for fast remediation.
- **Pacu** – Red team framework for simulating real-world AWS attacks.
- **Terraform** – Used to spin up a vulnerable AWS lab for testing.
- **Kali Linux** – For manual security testing.
- **Docker** – Containerizes workloads and tools.
- **Custom Python Scripts** – End-to-end automation of attack paths and alerts.

---

## 🧱 System Design Highlights

1. **Cloud Integration**:
   - Collects logs and telemetry from AWS, Azure, and GCP.
   - Uses those inputs for network monitoring and attack detection.

2. **CI/CD Pipeline Security**:
   - Automatically scans:
     - Container images
     - IaC configurations
     - Code commits
   - Plug-and-play with Jenkins, GitHub Actions, etc.

3. **Posture Management**:
   - Continuously checks for misconfigurations and non-compliance.
   - Built-in policies aligned with standards like GDPR, HIPAA, CIS.

4. **Runtime Monitoring**:
   - Watches for suspicious network flows or privilege misuse.
   - Detects lateral movement and real-time attacks (e.g., SSH brute force, Log4Shell).

---

## 🔍 Implementation Walkthrough

### 🛠️ Custom IAM Access Analyzer
A CLI tool built to deeply inspect AWS IAM policies. Key features:
- Detects insecure or over-privileged roles.
- Estimates cost of checks.
- Validates region-specific policy rules.

### 🔐 Setting Up the Vulnerable Lab
- Built using Terraform.
- Emulates a vulnerable AWS Lambda environment.
- Public-facing services, exposed roles, and weak IAM setups.

### ⚔️ Attack Simulation (End-to-End)
1. **Start with IAM enumeration** using Pacu and AWS CLI.
2. **Privilege escalation** via `assume-role`.
3. **SQL Injection** in a Lambda function exploited to attach `AdministratorAccess`.
4. **Session hijacking** using `swap_keys`.
5. **Secrets exfiltration** from S3 buckets and other sensitive APIs.

An **automated Python script** ties all of this together, simulating the attack path and logging events.

---

## ✅ Results

- **Multiple vulnerabilities** identified and confirmed, including misconfigured IAM roles and a working SQL injection.
- Seamlessly integrated with CI/CD for **shift-left security**.
- Demonstrated **real-time detection** of brute force and lateral movement attacks.
- Provided **detailed remediation output** and alerts.
- Enabled deep visibility into cloud activity without disrupting workflows.

---

## 📈 Takeaways

Raptor proves that strong cloud security doesn’t require bloated enterprise tools. With the right integrations and automation, security can be embedded directly into the development process—early, consistently, and intelligently.

---

## 🚀 What's Next?

- Add **machine learning** to predict future threats and detect anomalies.
- Extend compatibility to **multi-cloud environments**.
- Improve **UI/UX** so it’s usable by non-security teams.
- Scale for **larger enterprise deployments** with SOC/SIEM integration.

