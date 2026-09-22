# SOC Home Lab

I built this hands-on SOC home lab to practice real-world threat detection, log analysis, and incident response in a controlled environment.

## Overview

The setup centers on a Wazuh SIEM monitoring a Windows endpoint that I tuned with Sysmon to capture high-fidelity telemetry. To test the pipeline, I use Kali Linux to run controlled attack simulations, allowing me to analyze how malicious activity looks at the log level, write detection rules, and map alerts directly to MITRE ATT&CK.

## Architecture
<img width="2000" height="1414" alt="architecture " src="https://github.com/user-attachments/assets/f2069641-6d5e-46f1-988d-10a3e9a460c8" />

## Tools Used
- **Wazuh SIEM**
- **Windows 11 Endpoint**
  - [Sysmon](configurations/01-sysmon-tuning.md) (configured with Olaf Hartong's Sysmon-Modular for enhanced telemetry)
  - Windows Event Logs
  - Atomic Red Team
- **Kali Linux**
- **VirtualBox**

## Skills Demonstrated
- SIEM Monitoring using Wazuh
- Windows Event Log Analysis
- Sysmon Telemetry Analysis
- Security Alert Investigation
- Incident Detection and Triage
- Threat Detection and Analysis
- MITRE ATT&CK Mapping
- Endpoint Monitoring
- Log Correlation and Analysis
- Security Documentation
- Basic Threat Hunting
- Incident Reporting
