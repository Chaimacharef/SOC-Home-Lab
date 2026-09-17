# Phase 1: Endpoint Telemetry Enhancement with Sysmon & Olaf Hartong's Modular Config

## 1. Executive Summary
Default Windows Event Logging provides limited visibility into low-level host activity, often missing critical process injection, memory access, and command-line execution details. To build a detection-ready environment, **System Monitor (Sysmon v15.22)** was deployed on the Windows endpoint and tuned using **Olaf Hartong’s `sysmon-modular`** configuration ruleset. Telemetry streams were subsequently integrated into the **Wazuh SIEM** pipeline for centralized alerting.

---

## 2. Infrastructure & Tooling
* **Target OS:** Windows 10/11 Endpoint
* **Telemetry Agent:** Microsoft Sysinternals Sysmon
* **Configuration:** [Olaf Hartong sysmon-modular](https://github.com/olafhartong/sysmon-modular)
* **SIEM / Forwarder:** Wazuh Agent (`ossec.conf`)

---

## 3. Implementation Steps

### Step 3.1 — Modular Configuration Deployment
1. Downloaded Sysmon v15.22 from Sysinternals and extracted binaries to `C:\Sysmon`.
2. Fetched the compiled `sysmonconfig-modular.xml` directly from Olaf Hartong's repository.
3. Applied the modular configuration schema (`v4.91`) to the active Sysmon service via PowerShell:

```powershell
Set-Location C:\Sysmon
.\Sysmon64.exe -c .\sysmonconfig-modular.xml -accepteula
