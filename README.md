# 🛡️ End-to-End SOC Simulation Project

<p align="center">

![Platform](https://img.shields.io/badge/Platform-Windows_10-0078D6?style=for-the-badge&logo=windows)
![Attacker](https://img.shields.io/badge/Attacker-Kali_Linux-557C94?style=for-the-badge&logo=kalilinux)
![SIEM](https://img.shields.io/badge/SIEM-Wazuh-005571?style=for-the-badge)
![Telemetry](https://img.shields.io/badge/Telemetry-Sysmon-blue?style=for-the-badge)
![Status](https://img.shields.io/badge/Status-In_Progress-orange?style=for-the-badge)

</p>

---

# 📖 Overview

This repository documents my **End-to-End Security Operations Center (SOC) Simulation Project**, built to simulate realistic cyber attacks in a controlled lab environment while developing practical Blue Team skills.

The project covers the complete SOC workflow—from attack simulation and log collection to threat detection, alert investigation, IOC analysis, MITRE ATT&CK mapping, and incident documentation.

Rather than focusing on offensive exploitation, this project emphasizes **Detection Engineering**, **Threat Hunting**, and **SOC Analyst methodologies** using enterprise security monitoring tools.

---

# 🎯 Project Goals

- Build an enterprise-like SOC laboratory using virtual machines.
- Simulate realistic cyber attack scenarios.
- Collect Windows telemetry using Sysmon.
- Centralize logs with Wazuh SIEM.
- Detect malicious activities using Wazuh detection rules.
- Perform alert triage and incident investigation.
- Map attacks to the MITRE ATT&CK Framework.
- Produce professional SOC documentation suitable for a cybersecurity portfolio.

---

# 🏗️ Lab Architecture

```text
                           +----------------------+
                           |     Kali Linux       |
                           |  Attack Simulation   |
                           +----------+-----------+
                                      |
                                      |
                              Host-Only Network
                                      |
          +---------------------------+---------------------------+
          |                                                       |
          |                                                       |
+---------+---------+                                 +-----------+----------+
|    Windows 10     |                                 |    Ubuntu Server     |
|-------------------|                                 |----------------------|
| Wazuh Agent       | ------------------------------> | Wazuh Manager        |
| Sysmon            |                                 | Wazuh Dashboard      |
| Windows Defender  |                                 | Detection Engine     |
+-------------------+                                 +----------------------+
```

---

# ⚙️ Technology Stack

| Technology        | Purpose                                 |
| ----------------- | --------------------------------------- |
| Windows 10        | Target Endpoint                         |
| Kali Linux        | Attack Simulation                       |
| Ubuntu Server     | SIEM Infrastructure                     |
| Wazuh             | Security Information & Event Management |
| Sysmon            | Endpoint Telemetry                      |
| Windows Event Log | Native Security Logging                 |
| Oracle VirtualBox | Virtualization Platform                 |
| MITRE ATT&CK      | Threat Mapping Framework                |

---

# 🛠️ Skills Demonstrated

- SOC Monitoring
- Security Event Analysis
- Windows Event Log Analysis
- Sysmon Analysis
- Threat Hunting
- Detection Engineering
- Wazuh Administration
- IOC Analysis
- MITRE ATT&CK Mapping
- Security Investigation
- Endpoint Monitoring
- Blue Team Operations

---

# 📚 Attack Scenarios

| No  | Scenario                        | MITRE ATT&CK | Status         |
| --- | ------------------------------- | ------------ | -------------- |
| 01  | SMB Brute Force Login Detection | T1110        | ✅ Completed   |
| 02  | PowerShell Malware Execution    | T1059.001    | 🚧 In Progress |
| 03  | Windows Persistence             | T1547        | ⏳ Planned     |
| 04  | Privilege Escalation            | T1068        | ⏳ Planned     |
| 05  | Credential Dumping              | T1003        | ⏳ Planned     |
| 06  | Defense Evasion                 | T1562        | ⏳ Planned     |
| 07  | Scheduled Task Persistence      | T1053        | ⏳ Planned     |
| 08  | Ransomware Behavior Simulation  | T1486        | ⏳ Planned     |
| 09  | Threat Hunting Investigation    | Multiple     | ⏳ Planned     |
| 10  | Incident Response Report        | Multiple     | ⏳ Planned     |

---

# 🎯 Detection Workflow

```text
Attack Simulation
        │
        ▼
Windows Security Log / Sysmon
        │
        ▼
Wazuh Agent
        │
        ▼
Wazuh Manager
        │
        ▼
Detection Rules
        │
        ▼
Security Alert
        │
        ▼
Threat Hunting
        │
        ▼
SOC Investigation
```

---

# 🗺️ MITRE ATT&CK Coverage

| Tactic               | Technique                             | Technique ID | Status |
| -------------------- | ------------------------------------- | ------------ | ------ |
| Credential Access    | Brute Force                           | T1110        | ✅     |
| Execution            | PowerShell                            | T1059.001    | ⏳     |
| Persistence          | Registry Run Keys / Startup Folder    | T1547        | ⏳     |
| Privilege Escalation | Exploitation for Privilege Escalation | T1068        | ⏳     |
| Credential Access    | OS Credential Dumping                 | T1003        | ⏳     |
| Defense Evasion      | Impair Defenses                       | T1562        | ⏳     |
| Persistence          | Scheduled Task                        | T1053        | ⏳     |

---

# 📈 Project Progress

- [x] Lab Infrastructure
- [x] Wazuh Installation
- [x] Wazuh Agent Deployment
- [x] Sysmon Installation
- [x] Baseline Monitoring
- [x] Scenario 01 – SMB Brute Force Login
- [x] Scenario 02 – PowerShell Execution Detection
- [ ] Scenario 03 – Windows Persistence
- [ ] Scenario 04 – Privilege Escalation
- [ ] Scenario 05 – Credential Dumping
- [ ] Scenario 06 – Defense Evasion
- [ ] Scenario 07 – Scheduled Tasks
- [ ] Scenario 08 – Ransomware Simulation
- [ ] Scenario 09 – Threat Hunting
- [ ] Scenario 10 – Incident Response Report

---

# 📂 Repository Structure

```text
SOC-Simulation/
│
├── README.md
│
├── docs/
│   ├── 01-Brute-Force-Login/
│   ├── 02-PowerShell-Malware-Execution/
│   ├── 03-Windows-Persistence/
│   ├── 04-Privilege-Escalation/
│   ├── 05-Credential-Dumping/
│   ├── 06-Defense-Evasion/
│   ├── 07-Scheduled-Tasks/
│   ├── 08-Ransomware-Simulation/
│   ├── 09-Threat-Hunting/
│   └── 10-Incident-Response/
│
├── architecture/
│
├── detection-rules/
│
├── scripts/
│
├── images/
│
└── LICENSE
```

---

# 🧾 Documentation

Each attack scenario contains:

- Objective
- Lab Configuration
- Attack Simulation
- Detection Process
- Event Analysis
- Wazuh Alerts
- Indicators of Compromise (IOC)
- MITRE ATT&CK Mapping
- Investigation
- Lessons Learned
- Evidence (Screenshots & Logs)

---

# 📸 Project Gallery

> Screenshots will be added as each scenario is completed.

- Wazuh Dashboard
- Threat Hunting
- Windows Event Viewer
- Sysmon Logs
- Attack Simulation
- Detection Workflow

---

# 🎓 Learning Outcomes

Throughout this project I practice:

- Building a SOC lab from scratch
- Deploying Wazuh SIEM
- Configuring endpoint telemetry
- Investigating Windows Security Events
- Analyzing Sysmon logs
- Threat Hunting using Wazuh
- Security Alert Triage
- IOC Analysis
- MITRE ATT&CK Mapping
- Detection Engineering
- Professional Security Documentation

---

# 📚 References

- MITRE ATT&CK Framework
- Wazuh Documentation
- Microsoft Security Auditing Documentation
- Microsoft Sysmon Documentation
- Sigma Detection Rules
- OWASP Testing Guide

---

# 👨‍💻 Author

**Mhd. Jodiandri**

Cybersecurity Portfolio Project

Focused on:

- Security Operations Center (SOC)
- Blue Team Operations
- Threat Detection
- Threat Hunting
- Detection Engineering
- Incident Response

---

> ⭐ This repository is continuously updated as new attack scenarios are completed and investigated.

```

```
