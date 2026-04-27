# splunk-siem-homelab
Splunk SIEM Home Lab — Threat Detection, SPL Queries, SOC Dashboard &amp; Incident Response
# 🔐 Splunk SIEM Home Lab — Threat Detection & Incident Response

![Splunk](https://img.shields.io/badge/Splunk-Enterprise%2010.2.2-black?style=flat&logo=splunk)
![Ubuntu](https://img.shields.io/badge/Ubuntu-24.04%20LTS-orange?style=flat&logo=ubuntu)
![MITRE](https://img.shields.io/badge/MITRE-ATT%26CK%20Mapped-red?style=flat)
![Security+](https://img.shields.io/badge/CompTIA-Security%2B-red?style=flat)
![Status](https://img.shields.io/badge/Status-Complete-green?style=flat)

## 📌 Project Overview

Built a fully functional enterprise-grade SOC home lab using Splunk Enterprise 
on Ubuntu Server 24.04 LTS. Ingested 35,000+ real Windows Security Event logs, 
wrote SPL detection queries mapped to MITRE ATT&CK, identified real suspicious 
accounts with elevated privileges, and built a 5-panel SOC dashboard.

> This lab mirrors real-world SOC operations performed at Sun Life Financial 
> and Rogers Communications.

---

## 🏗️ Lab Architecture
Windows 11 Host (ASUS TUF Gaming F15)
│
├── Oracle VirtualBox 7.2.8
│       └── Ubuntu Server 24.04.4 LTS VM
│               └── Splunk Enterprise 10.2.2
│                       └── Listening on port 9997
│
└── Splunk Universal Forwarder
└── Forwarding Windows Security Events
└── Index: wineventlog
---

## 🛠️ Tools & Technologies

| Category | Tools |
|---|---|
| SIEM Platform | Splunk Enterprise 10.2.2 |
| Server OS | Ubuntu Server 24.04.4 LTS |
| Hypervisor | Oracle VirtualBox 7.2.8 |
| Log Forwarder | Splunk Universal Forwarder |
| Host OS | Windows 11 Home (Build 26200) |
| Framework | MITRE ATT&CK |
| Language | SPL (Splunk Processing Language) |

---

## 📊 SOC Dashboard

![SOC Dashboard](screenshots/soc-dashboard-complete.png)

*5-panel SOC Security Overview Dashboard monitoring 9,598 security events*

---

## 🔍 Key Findings

| Finding | Severity | MITRE Technique |
|---|---|---|
| niroco service — SeImpersonatePrivilege | 🔴 HIGH | T1078, T1134 |
| 1kClassAds adware with admin rights | 🟠 MEDIUM | T1078 |
| Failed login attempts (PROSYSTEM$) | 🟡 LOW | T1110 |
| High volume Credential Manager access | 🔵 INFO | T1555 |

---

## 🎯 SPL Detection Queries

### Brute Force Detection (T1110)
```spl
index=wineventlog EventCode=4625
| stats count by Account_Name
| sort -count
```

### Privilege Escalation (T1078, T1134)
```spl
index=wineventlog EventCode=4672
| stats count by Account_Name
| sort -count
```

### New Account Creation (T1136)
```spl
index=wineventlog EventCode=4720
| table _time, Account_Name, host
| sort -_time
```

### Suspicious Process Execution (T1059)
```spl
index=wineventlog EventCode=4688
| table _time, Account_Name, New_Process_Name
| sort -_time
```

### Security Events Timeline
```spl
index=wineventlog
| timechart count span=1h
```

---

## 📸 Screenshots

### Splunk Receiving Windows Logs
![Logs](screenshots/windows-logs-in-splunk.png)

### Privilege Escalation Detection
![Privilege](screenshots/privilege-escalation-detection.png)

### niroco Investigation
![niroco](screenshots/niroco-investigation.png)

### Failed Login Detection
![Failed Logins](screenshots/failed-login-detection.png)

---

## 📋 MITRE ATT&CK Coverage

| Technique | Name | Detected |
|---|---|---|
| T1078 | Valid Accounts | ✅ |
| T1134 | Access Token Manipulation | ✅ |
| T1110 | Brute Force | ✅ |
| T1555 | Credentials from Password Stores | ✅ |
| T1059 | Command and Scripting Interpreter | ✅ |
| T1136 | Create Account | ✅ Monitored |

---

## 📁 Repository Structure
splunk-siem-homelab/
├── README.md
├── detections/
│   ├── brute-force-4625.spl
│   ├── privilege-escalation-4672.spl
│   ├── new-account-creation-4720.spl
│   └── process-execution-4688.spl
├── reports/
│   └── Ganesh_IR_Report_Splunk_SIEM_Lab.docx
└── screenshots/
├── soc-dashboard-complete.png
├── windows-logs-in-splunk.png
├── privilege-escalation-detection.png
├── niroco-investigation.png
└── failed-login-detection.png
---

## 📄 Incident Response Report

Full IR report available in `/reports/` folder including:
- Executive Summary
- Timeline of events
- All findings with risk ratings
- MITRE ATT&CK mapping
- Remediation recommendations

---

## 👨‍💻 Author

**Sree Ganesh Madhusoodhanan**
Cyber Security Engineer | CompTIA Security+ | Splunk Core Certified
Sun Life Financial — Toronto, Ontario, Canada

---

## 🏷️ Tags

`splunk` `siem` `soc` `threat-detection` `mitre-attack` `incident-response` 
`cybersecurity` `blue-team` `spl` `windows-event-logs` `ubuntu` `homelab`
