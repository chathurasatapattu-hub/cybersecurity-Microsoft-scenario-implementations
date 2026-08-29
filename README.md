# Microsoft Cloud Security Implementations: Real-World Scenarios

![Cloud](https://img.shields.io/badge/Cloud-Microsoft_365-0078D4.svg)
![Identity](https://img.shields.io/badge/Identity-Entra_ID-00599C.svg)
![Security](https://img.shields.io/badge/Security-Zero_Trust-black.svg)
![Status](https://img.shields.io/badge/Status-Active_Development-success.svg)

This repository contains detailed technical documentation, architectural decisions, and troubleshooting logs for deploying enterprise-grade security controls within a Microsoft 365 and Entra ID lab environment. 

Rather than just following basic configuration guides, this project demonstrates a **Zero Trust approach** to identity access management, privileged role elevation, and endpoint security, complete with real-world troubleshooting and business-aligned outcomes.

---

## 📂 Repository Structure

```text
cybersecurity-Microsoft-scenario-implementations/
├── README.md
├── lessons-learned/
│   └── Lessons_Learned.md                 # Master log of architectural insights and troubleshooting
├── Identity-Scenario-1/
│   ├── docs/                              # Executive summaries and technical runbooks
│   └── assets/                            # Sanitized configuration evidence
├── Identity-Scenario-2/
│   ├── docs/                              # JIT admin and offboarding documentation
│   └── assets/                            # Evidence of PIM and SharePoint access controls
└── Endpoint-Scenario-1/                   # [IN PROGRESS] Microsoft Defender & Intune configurations
