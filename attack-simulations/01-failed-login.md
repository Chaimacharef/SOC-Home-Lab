# Attack Simulation 1: Failed Login 

## Objective
Detect and investigate failed authentication attempts on a Windows endpoint using Wazuh SIEM.

## Investigation Process 
Manually generated failed authentication attempts to trigger Windows Security Event ID 4625 and corresponding Wazuh SIEM alerts.


<img width="2560" height="1440" alt="VirtualBox_Windows11_04_08_2026_15_52_05" src="https://github.com/user-attachments/assets/7f877c9b-2de0-425d-acb3-2bfba8f72da8" />


Reviewed Windows Event Viewer to identify the generated authentication failures. Event ID 4625 confirmed that the failed login attempts were successfully recorded.


<img width="2560" height="1440" alt="VirtualBox_Windows11_04_08_2026_16_12_31" src="https://github.com/user-attachments/assets/9b118632-6cf6-4ad6-a9e9-24c6079c397d" />


The event details were examined to identify the affected account, logon type, and failure reason. These fields were used to validate the activity and correlate it with SIEM telemetry.

| Field | Details |
|---|---|
| Event ID | 4625 - Failed Logon Attempt |
| Account Name | root |
| Logon Type | 2 - Interactive |
| Failure Reason | Unknown user name or bad password |
| Status Code | 0xC000006D |

Wazuh successfully ingested the Windows Security event and generated an alert. The alert metadata, severity, and associated event information were reviewed to validate successful detection.


<img width="2560" height="1440" alt="VirtualBox_Windows11_04_08_2026_16_06_10" src="https://github.com/user-attachments/assets/7d165e8e-b092-4807-9e46-0b98ee9e8d11" />


<img width="2560" height="1440" alt="VirtualBox_Windows11_04_08_2026_16_07_10" src="https://github.com/user-attachments/assets/e3bcd8c7-8994-43e9-8367-379edeca6b55" />


<img width="2560" height="1440" alt="VirtualBox_Windows11_04_08_2026_16_09_12" src="https://github.com/user-attachments/assets/3849b625-be40-44f5-8620-5d626c30c607" />


## MITRE ATT&CK Mapping
- **Technique:** T1110 Brute Force
- **Tactic:** Credential Access

## Findings
✅ Failed authentication activity was successfully simulated on the Windows endpoint.  
✅ Windows Security Event ID 4625 was generated and captured.  
✅ Event data was successfully ingested and monitored through Wazuh SIEM.  
✅ Detection and log correlation were validated through SIEM analysis.

## Defense Recommendations
1. Configure account lockout policies to limit repeated failed authentication attempts.
2. Enable Multi-Factor Authentication (MFA) for user accounts where possible.
3. Monitor repeated Event ID 4625 failures for potential brute-force activity.
4. Investigate repeated authentication failures from the same user or source.
6. Review and tune SIEM alert thresholds to detect suspicious login patterns.

