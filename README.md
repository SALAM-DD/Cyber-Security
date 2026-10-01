arkdown
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

PowerShell
Get-Service Sysmon64
2. Splunk Data Ingestion Configuration
Configured Splunk's local data input definitions (C:\Program Files\Splunk\etc\system\local\inputs.conf) to monitor the Sysmon operational channel:

Ini, TOML
[WinEventLog://Microsoft-Windows-Sysmon/Operational]
disabled = 0
renderXml = true
index = main
3. Lab Optimization & Service Management
Configured disk usage threshold overrides (minFreeSpace = 500) in server.conf to optimize performance in low-capacity laboratory environments.

Applied changes and restarted the core indexing service:

PowerShell
net stop Splunkd ; net start Splunkd
🧪 Verification & Telemetry Validation
Test Payload Execution
To confirm telemetry ingestion without noise, a benign command-line process creation was executed on the host endpoint:

PowerShell
powershell.exe -Command "Write-Host 'SOC-Inspection-Test'"
SPL Verification Query
Inside Splunk Web (Search & Reporting), executed the following Search Processing Language (SPL) query:

Splunk SPL
index=main "SOC-Inspection-Test"
Log Ingestion Proof
The search successfully returned the process creation telemetry, capturing key metadata fields including Image, CommandLine, ParentCommandLine, User, and ProcessId.

🎯 Key Takeaways & Learned Skills
Endpoint Telemetry Setup: Deepened understanding of Sysmon driver service architecture and Windows Event Log channels.


SIEM Ingestion Pipelines: Mastered direct file-based configuration (inputs.conf, server.conf) in enterprise Splunk environments.

Troubleshooting & Engineering: Diagnosed and resolved architecture mismatch issues (x64 vs. ARM64) and storage quota constraints in local testing environments.

<img width="1920" height="1078" alt="Search _ Splunk 10 4 4 - Personal - Microsoft​ Edge 01_10_2026 01_20_24" src="https://github.com/user-attachments/assets/4cd8c135-c4d5-482a-824f-925fedafc030" />
<img width="1481" height="761" alt="Administrator_ Windows PowerShell 01_10_2026 01_20_42" src="https://github.com/user-attachments/assets/0f745153-a200-40b2-bb18-baf74e0ae6d3" />
