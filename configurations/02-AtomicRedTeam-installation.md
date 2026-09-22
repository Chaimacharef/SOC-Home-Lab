<div align="center"> <img src="https://github.com/user-attachments/assets/2cbd7066-ce9c-4c00-b02b-517310a2e542"> </div> 

---

To test my SIEM detection rules and telemetry pipeline, I installed **Atomic Red Team** on my Windows endpoint. It provides small, focused test scripts mapped directly to the MITRE ATT&CK framework. This allows me to simulate real adversary techniques in a controlled environment and verify that Sysmon and Wazuh catch the activity.

## 1. Temporarily Adjust PowerShell Execution Policy

I first changed the PowerShell execution policy for my current session:

```powershell
Set-ExecutionPolicy Bypass -Scope Process -Force
```

I used `-Scope Process` so the change only applies to the PowerShell window I was working in. It does not permanently change the system policy.

I also used `-Force` so PowerShell would not ask for confirmation.

---

## 2. Configure Microsoft Defender

Before running the Atomic Red Team tests, I added the Atomic Red Team folder as a Microsoft Defender exclusion:

```powershell
Add-MpPreference -ExclusionPath "C:\AtomicRedTeam"
```

Atomic Red Team tests can sometimes be detected by Defender because some of the files and scripts are designed to simulate real attack techniques.

I added the exclusion so Defender would not remove the test files before I could run them and collect the activity in Wazuh.

> **Lab note:** I only made this change on my isolated Windows lab VM because excluding a folder from Defender reduces its protection.

---

## 3. Install Invoke-AtomicRedTeam

Next, I installed **Invoke-AtomicRedTeam** and downloaded the Atomic Red Team test library:

```powershell
IEX (IWR 'https://raw.githubusercontent.com/redcanaryco/invoke-atomicredteam/master/install-atomicredteam.ps1' -UseBasicParsing); Install-AtomicRedTeam -GetAtomics
```

This command downloads the installation script, runs it, and downloads the Atomic tests to my Windows VM.

### `IWR` — Invoke-WebRequest

I used `IWR` to download the installation script from the Red Canary GitHub repository:

```powershell
IWR 'https://raw.githubusercontent.com/redcanaryco/invoke-atomicredteam/master/install-atomicredteam.ps1' -UseBasicParsing
```

The `-UseBasicParsing` option was used to handle the web request using basic parsing.

### `IEX` — Invoke-Expression

I then used `IEX` to run the downloaded installation script:

```powershell
IEX (...)
```

### Install-AtomicRedTeam

`Install-AtomicRedTeam` sets up the framework that I use to run the Atomic Red Team tests.

### `-GetAtomics`

I used `-GetAtomics` to download the Atomic Red Team test library to my Windows VM.

The tests were stored in:

```text
C:\AtomicRedTeam\atomics
```

The library contains tests mapped to different **MITRE ATT&CK techniques**, for example:

```text
T1059.001 - PowerShell
T1003.001 - LSASS Memory
```

This allows me to choose a specific technique, run the corresponding test, and then look at the activity generated in Wazuh.

---

## 4. Verify the Installation

After the installation finished, I checked the PowerShell output to make sure Invoke-AtomicRedTeam was installed correctly.

I received the following message:

```text
Installation of Invoke-AtomicRedTeam is complete. You can now use the Invoke-AtomicTest function
```

This confirmed that the installation was successful and that I could use `Invoke-AtomicTest` to run the tests.

---

## Result

At this point, my Windows endpoint was ready for the next stage of the lab.

<img width="2560" height="687" alt="VirtualBox_Windows11_20_09_2026_14_36_25" src="https://github.com/user-attachments/assets/0628a28a-947f-4c73-af91-66a9329333fc" />
