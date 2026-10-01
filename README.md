# Project 1: Windows Home Lab & SIEM Deployment (Splunk + Sysmon)

## 📌 Executive Summary
This project demonstrates the design, deployment, and validation of a Windows-focused Security Information and Event Management (SIEM) telemetry pipeline using **Splunk Enterprise** and **Microsoft Sysmon**. 

The goal of this home lab environment is to establish granular endpoint visibility and ingest critical process creation telemetry (`Event ID 1`) into Splunk for active threat detection and SOC monitoring workflows.

---

## 🏗️ Lab Architecture & Telemetry Flow
[ Local Windows Endpoint ]
│
├──► Microsoft Sysmon (x64) ──► Event ID 1 (Process Creation)
│                                        │
└──► Local Splunk Enterprise ◄───────────┘
(inputs.conf Engine / WinEventLog Channel)
- **Host OS**: Windows 10/11 (x64 Architecture)
- **SIEM Platform**: Splunk Enterprise `v9.x`
- **Telemetry Sources**:
  - `WinEventLog:Microsoft-Windows-Sysmon/Operational`
  - Windows Security Logs (`Security`, `System`, `Application`)

---

## 🛠️ Implementation & Configuration Steps

### 1. Sysmon Telemetry Agent Installation
- Downloaded and verified standard 64-bit binaries for host architecture.
- Installed the Sysmon Windows service with administrative rights and standard configuration:
  ```powershell
  .\Sysmon64.exe -accepteula -i
 Verified service registration and execution status:
    .Get-Service Sysmon64

--Splunk Data Ingestion Configuration
Configured Splunk's local data input definitions (C:\Program Files\Splunk\etc\system\local\inputs.conf) to monitor the Sysmon operational channel:
[WinEventLog://Microsoft-Windows-Sysmon/Operational]
disabled = 0
renderXml = true
index = main
--Lab Optimization & Service Management
Configured disk usage threshold overrides (minFreeSpace = 500) in server.conf to optimize performance in low-capacity laboratory environments.

Applied changes and restarted the core indexing service:

PowerShell
net stop Splunkd ; net start Splunkd
--🧪 Verification & Telemetry Validation
Test Payload Execution
To confirm telemetry ingestion without noise, a benign command-line process creation was executed on the host endpoint:

PowerShell
powershell.exe -Command "Write-Host 'SOC-Inspection-Test'"
