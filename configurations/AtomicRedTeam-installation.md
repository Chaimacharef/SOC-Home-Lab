## 📌 Overview
To conduct controlled threat simulations and test telemetry coverage, I deployed **Atomic Red Team** on the Windows endpoint. Atomic Red Team provides small, highly targeted test scripts mapped directly to the MITRE ATT&CK framework, allowing me to simulate adversary behavior and verify if Sysmon and Wazuh capture the activity.

## 1. Prerequisites & Defender Configuration
Because Atomic Red Team utilizes real adversary technique scripts, endpoint security controls (like Windows Defender) can block installation or delete test payloads. To prevent this during lab testing, I configured a dedicated execution exclusion path.

## 2. Installation Steps

Running PowerShell as Administrator, set the execution policy, configured the Defender exclusion, and executed the automated installation script:

```powershell
# 1. Set execution policy for the session
Set-ExecutionPolicy Bypass -Scope Process -Force

# 2. Add Windows Defender exclusion for the installation folder
Add-MpPreference -ExclusionPath "C:\AtomicRedTeam"

# 3. Download and install Invoke-AtomicRedTeam framework
IEX (IWR '[https://raw.githubusercontent.com/redcanaryco/invoke-atomicredteam/master/install-atomicredteam.ps1](https://raw.githubusercontent.com/redcanaryco/invoke-atomicredteam/master/install-atomicredteam.ps1)' -UseBasicParsing); Install-AtomicRedTeam -GetAtomics
