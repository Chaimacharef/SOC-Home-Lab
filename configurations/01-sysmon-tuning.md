## 📌 Overview
By default, Windows Sysmon generates thousands of noisy, routine system logs every hour—making it easy for actual malicious activity to get lost in the clutter. 

This repository documents how I configured and tuned Microsoft Sysmon using **Olaf Hartong’s `sysmon-modular` framework**. The goal of this project is to filter out harmless background OS noise, reduce SIEM storage bloat, and ensure high-fidelity telemetry is captured for critical attack techniques (like process injection, credential dumping, and registry persistence).

## 1. Infrastructure & Tooling
* **Target OS:** Windows 11 Endpoint
* **Telemetry Agent:** Microsoft Sysinternals Sysmon
* **Configuration:** [Olaf Hartong sysmon-modular](https://github.com/olafhartong/sysmon-modular)
* **SIEM / Forwarder:** Wazuh Agent (`ossec.conf`)

## 2. Implementation Steps

### 1. Download Modular Configuration

Created the working directory and fetched the latest `sysmonconfig-modular.xml` configuration directly from Olaf Hartong's repository via PowerShell:

```powershell
New-Item -ItemType Directory -Path "C:\Sysmon" -Force
Invoke-WebRequest -Uri "https://raw.githubusercontent.com/olafhartong/sysmon-modular/master/sysmonconfig.xml" -OutFile "C:\Sysmon\sysmonconfig-modular.xml"
```

<img width="2560" height="617" alt="Sysmon command" src="https://github.com/user-attachments/assets/13801d8d-1163-493b-afd1-c55a893efc2f" />

### 2. Install / Apply the Modular Configuration

Next, apply the downloaded `sysmonconfig-modular.xml` configuration file to Sysmon using PowerShell:

```powershell
Set-Location C:\Sysmon
.\Sysmon64.exe -c .\sysmonconfig-modular.xml -accepteula
```

<img width="2560" height="586" alt="VirtualBox_Windows11_17_09_2026_18_43_12" src="https://github.com/user-attachments/assets/18ab1c0d-018e-47f0-83d8-00c4bc8494e3" />

### 3. Verify Configuration Load (Event ID 16)
Verified in Windows Event Viewer (Microsoft-Windows-Sysmon/Operational) that Event ID 16 appeared, confirming Sysmon successfully reloaded the modular configuration file:

<img width="2560" height="1374" alt="VirtualBox_Windows11_17_09_2026_18_47_22" src="https://github.com/user-attachments/assets/0c228874-88e9-4a0a-b12b-2c08ecd9f381" />

### 4. Configure Wazuh Agent Log Ingestion
Edited the Wazuh Agent configuration file (`C:\Program Files (x86)\ossec-agent\ossec.conf`) to collect Sysmon telemetry and forward it to the SIEM:

```xml
<localfile>
  <location>Microsoft-Windows-Sysmon/Operational</location>
  <log_format>eventchannel</log_format>
</localfile>
```

<img width="2560" height="1393" alt="VirtualBox_Windows11_17_09_2026_18_49_51" src="https://github.com/user-attachments/assets/8bb66bcd-79a2-42dc-973c-aed48e87d565" />
