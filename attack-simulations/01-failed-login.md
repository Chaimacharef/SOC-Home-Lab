# Attack Simulation 1: Failed Login 

## Objective
In this investigation, I wanted to test whether I could detect and investigate failed login attempts on a Windows endpoint using Wazuh SIEM.

The goal was to generate the activity, identify the Windows event it created, and then confirm that Wazuh was able to collect and alert on it.

## Investigation Process 
I manually generated several failed login attempts on the Windows VM to create Windows Security Event ID 4625.


<img width="2560" height="1440" alt="VirtualBox_Windows11_04_08_2026_15_52_05" src="https://github.com/user-attachments/assets/7f877c9b-2de0-425d-acb3-2bfba8f72da8" />

I then opened Windows Event Viewer to check whether the failed login attempts had been recorded.
I found Event ID 4625, confirming that Windows had successfully logged the failed authentication attempts.


<img width="2560" height="1440" alt="VirtualBox_Windows11_04_08_2026_16_12_31" src="https://github.com/user-attachments/assets/9b118632-6cf6-4ad6-a9e9-24c6079c397d" />


I reviewed the event details to understand what happened and to identify the affected account, logon type, and reason for the authentication failure.

| Field | Details |
|---|---|
| Event ID | 4625 - Failed Logon Attempt |
| Account Name | root |
| Logon Type | 2 - Interactive |
| Failure Reason | Unknown user name or bad password |
| Status Code | 0xC000006D |

then checked Wazuh to see whether the Windows event had been successfully collected and generated an alert.
Wazuh successfully received the event, and I reviewed the alert details, severity, and event information to make sure the activity was being detected correctly.


<img width="2560" height="1440" alt="VirtualBox_Windows11_04_08_2026_16_06_10" src="https://github.com/user-attachments/assets/7d165e8e-b092-4807-9e46-0b98ee9e8d11" />


<img width="2560" height="1440" alt="VirtualBox_Windows11_04_08_2026_16_07_10" src="https://github.com/user-attachments/assets/e3bcd8c7-8994-43e9-8367-379edeca6b55" />


<img width="2560" height="1440" alt="VirtualBox_Windows11_04_08_2026_16_09_12" src="https://github.com/user-attachments/assets/3849b625-be40-44f5-8620-5d626c30c607" />


## MITRE ATT&CK Mapping
- **Technique:** T1110 Brute Force
- **Tactic:** Credential Access

## Findings
✅ I successfully generated failed authentication activity on the Windows endpoint.

✅ Windows Security Event ID 4625 was generated and recorded.

✅ Wazuh successfully collected the Windows security event.

✅ Wazuh generated an alert for the activity.

✅ I was able to review and correlate the Windows event with the SIEM alert.

## Defense Recommendations
1. Configure account lockout policies to limit repeated failed authentication attempts.
2. Enable Multi-Factor Authentication (MFA) for user accounts where possible.
3. Monitor repeated Event ID 4625 failures for potential brute-force activity.
4. Investigate repeated authentication failures from the same user or source.
6. Review and tune SIEM alert thresholds to detect suspicious login patterns.

