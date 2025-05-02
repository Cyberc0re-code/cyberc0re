---
title: 'Raptor - Cloud Native Application Protection Platform'
published: 2023-12-22
description: 'Cloud Security Monitoring Tool'
image: './Cloud.png'
tags: [Cloud Security, Tool, Monitoring Tool]
category: 'Cloud'
draft: false
---

# Raptor – Cloud Native Application Protection Platform (CNAPP)

### [📄 View Full Project Report (PDF)](https://drive.google.com/file/d/17A0Spy1OBobnHelzcbWOctZBXWSMURxX/view?usp=sharing)

Note: This is a part of my college project.
## 🛡️ Overview

Raptor is a purpose-built Cloud Native Application Protection Platform (CNAPP) designed to provide real-time, integrated security for cloud-native applications deployed primarily on AWS. Unlike traditional perimeter-based security models, Raptor embeds protection mechanisms directly into development and deployment pipelines, offering end-to-end coverage across the CI/CD lifecycle. It is architected around open-source tooling and custom automation, aiming to lower the entry barrier for cloud security without compromising on depth or granularity.

At a time when microservices, containers, and ephemeral compute resources have become the norm, cloud environments are constantly shifting, making manual security processes ineffective. Raptor was developed to address this challenge by tightly integrating security into existing DevOps workflows, automating critical functions such as vulnerability scanning, misconfiguration detection, IAM policy validation, and runtime threat analysis. The system begins by ingesting data from cloud service APIs—such as AWS, Azure, and GCP—to gather logs, network flow records, and cloud resource metadata. This telemetry feeds into a centralized processing pipeline that performs behavioral analysis and policy enforcement.

Raptor’s first layer of defense lies in its deep CI/CD integration. It proactively scans application codebases, container images, and infrastructure-as-code (IaC) configurations as they move through deployment stages. Tools like **Prowler** are integrated to benchmark cloud environments against industry standards such as CIS and GDPR. For vulnerability intelligence, **Wiz open-cvdb** is leveraged to provide real-time CVE tracking and alerting. All of this is executed within isolated **Docker** containers, ensuring consistent testing environments and minimal impact on host systems.

A key technical innovation in Raptor is the custom-built IAM Access Analyzer—a CLI tool developed to parse, validate, and audit AWS IAM policies defined in CloudFormation templates. It extends AWS's native IAM analyzer capabilities by supporting region-specific validations, cost-aware execution logic, and granular permission inspection. The tool was designed to simulate adversarial scenarios where improper permissions could lead to privilege escalation, cross-account access, or resource hijacking. These capabilities are especially relevant in multi-region, production-grade AWS architectures.

To simulate real-world threats, a dedicated vulnerable AWS lab environment was deployed using **Terraform**. This lab includes public IAM roles, insecure Lambda functions, and loosely defined permissions. Within this setup, a comprehensive attack chain was executed using **Pacu**, a red team framework for AWS. The simulated attack began with IAM enumeration and role discovery, followed by privilege escalation via a SQL injection vulnerability in a Lambda function. This injection allowed the attacker to assign `AdministratorAccess` to their own identity and then hijack privileged sessions using the `swap_keys` technique. Subsequent stages included accessing S3 buckets containing sensitive cost data and downloading internal `.py` files used in production workloads.

To automate and validate the entire kill chain, a Python script was written to execute the attack path end-to-end. The script mimics real-world attacker behavior—confirming credentials, assuming roles, injecting payloads, elevating privileges, dumping secrets, and logging every action in structured JSON format for use in SIEMs or alerting pipelines. This automation not only enables faster testing but also acts as a detection blueprint for blue teams looking to simulate cloud-native threats in controlled environments.

Beyond point-in-time assessments, Raptor continuously monitors runtime behavior. It detects SSH brute-force attacks, lateral movement, and sensitive resource access attempts by analyzing network traffic and event logs in real time. Known vulnerabilities like **Log4Shell (CVE-2021-44228)** are identified during execution by scanning runtime binaries and logs. These features bridge the gap between static analysis and dynamic threat detection, offering both preventive and detective controls.

From a DevSecOps perspective, Raptor emphasizes usability without compromising security. Developers can deploy Raptor alongside existing CI/CD pipelines with minimal configuration overhead. Security becomes a shared responsibility—from code commit to cloud deployment. By offering automated validation, real-time alerts, and continuous compliance, Raptor empowers teams to shift security left and scale securely.

Ultimately, Raptor demonstrates that robust cloud-native security can be achieved without relying on heavyweight commercial suites. By combining custom security tooling, cloud exploitation frameworks, and automation-first design, the platform showcases how cloud environments can be made resilient against modern adversaries—right from within the DevOps toolchain.

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
## What's Next?

- Add **machine learning** to predict future threats and detect anomalies.
- Extend compatibility to **multi-cloud environments**.
- Improve **UI/UX** so it’s usable by non-security teams.
- Scale for **larger enterprise deployments** with SOC/SIEM integration.

