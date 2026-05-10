# 🔍 Nmap Reconnaissance Detection Lab

> **SOC Analysis Lab** | Splunk SIEM · Nmap · Windows Security Logs · SPL

---

## 📌 Overview

In this lab, I simulated a **network reconnaissance attack** using Nmap from a Kali Linux machine and built a full detection pipeline using **Splunk**. The goal was to replicate how a SOC Analyst detects and responds to port scanning and reconnaissance activity.

---

## 🎯 Objectives

- Simulate a realistic network reconnaissance attack using Nmap
- Configure Splunk Universal Forwarder to capture Windows Security Logs
- Develop SPL queries to detect scanning patterns based on Event Codes
- Build a dedicated SOC dashboard for real-time reconnaissance visibility

---

## 🧰 Tools Used

| Tool | Purpose |
|------|---------|
| **Nmap** | Attack simulation — network scanning and reconnaissance |
| **Kali Linux** | Attacker machine |
| **Splunk Enterprise** | SIEM platform for log ingestion and analysis |
| **Splunk Universal Forwarder** | Forwarding Windows Security Logs to Splunk |
| **Windows Security Logs** | Log source containing security events |
| **SPL** | Search Processing Language for threat detection |

---

## 🔬 Lab Architecture

```
[Kali Linux - Attacker]
        |
        | Nmap Port Scan / Reconnaissance
        ↓
[Windows Target Machine]
        |
        | Windows Security Logs → Splunk Universal Forwarder
        ↓
[Splunk Enterprise - SIEM]
        |
        | SPL Queries → Alerts → SOC Dashboard
```

---

## 🚀 Step-by-Step Walkthrough

### Step 1: Attack Simulation (Nmap)

Performed a comprehensive port scan from Kali Linux against a Windows target to generate security events.

```bash
# SYN Scan — stealthy and fast
nmap -sS TARGET_IP

# OS fingerprinting + service detection
nmap -A TARGET_IP

# Full port scan
nmap -p- TARGET_IP
```

**What this generates:** Multiple failed connection attempts and network events logged in Windows Security Logs.

---

### Step 2: Log Forwarding (Splunk Universal Forwarder)

Configured the **Splunk Universal Forwarder** on the Windows target to ship Security Logs to Splunk Enterprise in real time.

```ini
[WinEventLog://Security]
index = windows_logs
disabled = false
```

---

### Step 3: Detection with SPL

Developed custom **SPL queries** to identify reconnaissance patterns based on Windows Event Codes:

```spl
index=windows_logs EventCode=5156 OR EventCode=5157
| stats count by src_ip, dest_port, EventCode
| where count > 50
| sort - count
```

**Key Event Codes monitored:**

| Event Code | Description |
|------------|-------------|
| 5156 | Windows Filtering Platform permitted a connection |
| 5157 | Windows Filtering Platform blocked a connection |
| 4625 | Failed logon attempt |

---

### Step 4: SOC Dashboard

Built a dedicated **SOC dashboard** to monitor active reconnaissance including:

- Top scanning source IPs
- Most targeted ports
- Connection attempt timeline
- Blocked vs permitted traffic ratio

![Nmap Detection Dashboard](Screenshot%202026-05-04%20131009.png)

---

## 🔍 Key Findings

| Indicator | Value |
|-----------|-------|
| Scan type detected | SYN Scan / Full port scan |
| Detection method | High volume connection attempts per IP |
| Key Event Codes | 5156, 5157 |
| Time to detection | Real-time via Splunk dashboard |

---

## 🧠 MITRE ATT&CK Mapping

| Technique | ID | Description |
|-----------|----|-------------|
| Network Service Discovery | T1046 | Scanning ports to identify open services |
| Active Scanning | T1595 | Probing network infrastructure |
| OS Fingerprinting | T1592 | Gathering info about target OS and services |

---

## 📚 What I Learned

- How attackers use Nmap to map network infrastructure before an attack
- Configuring Splunk Universal Forwarder for Windows Security Log collection
- Writing SPL queries to detect scanning patterns using Event Codes
- Building SOC dashboards to monitor reconnaissance in real time
- Correlating network events to identify malicious behavior

---

## 👤 Author

**Abdelrahman Mohamed Hussein** — SOC Analyst  
[LinkedIn](https://www.linkedin.com/in/abd-el-rahman-mohamed-hussein-a6071b256) | [GitHub](https://github.com/abdelrahman-28m)
