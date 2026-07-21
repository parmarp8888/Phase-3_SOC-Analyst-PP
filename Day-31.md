
#  Walkthrough: [Windows Incident Surface](https://tryhackme.com/room/winincidentsurface)

As part of my **Day 0–100 SOC Analyst L1 Learning Journey**, I completed the **Windows Incident Surface** room to understand where attackers leave traces on a Windows system and how defenders can investigate them.

<img width="1200" height="1200" alt="image" src="https://github.com/user-attachments/assets/f40fbc91-b62d-46c0-8505-2474f00cea22" />

My objective was not simply to answer the room questions, but to understand **which Windows artifacts are valuable during an incident investigation**. Initially, I was confused by the large number of Windows components such as the Registry, Services, Scheduled Tasks, Startup entries, Event Logs, User Sessions, and Network Connections. They all seemed unrelated, but this room showed me that each artifact tells part of the attacker's story.

---


## Who is this for?

* SOC Analysts
* Incident Responders
* Digital Forensics Analysts
* Blue Team Professionals
* Windows System Administrators

## What is Windows Incident Surface?

Windows Incident Surface refers to the collection of Windows components and forensic artifacts that attackers commonly abuse and defenders investigate during a security incident.

## Where does this apply?

* Enterprise Windows workstations
* Windows Servers
* Active Directory environments
* SOC monitoring and Incident Response investigations

## When should I learn this?

This topic should be learned after understanding Windows fundamentals because it forms the basis of endpoint investigation and malware analysis.

## Why is it important?

Attackers rarely disappear without leaving evidence. Registry keys, services, startup folders, scheduled tasks, processes, event logs, and network connections often reveal how an attacker gained access, maintained persistence, and moved inside the environment.

## How does it work?

An investigator collects evidence from multiple Windows artifacts and correlates them to build a timeline of attacker activity instead of relying on a single indicator.

---


## What confused me?

The biggest challenge was understanding **where to investigate first**.

Questions that came to mind included:

* Why are attackers modifying the Windows Registry?
* Why would someone delete Windows Event Logs?
* Why are Services and Scheduled Tasks frequently abused?
* How are suspicious processes connected to persistence?

At first, these artifacts looked like independent components. After completing the room, I realized they are connected and collectively describe an attack lifecycle.

## What I learned?

This room introduced several important Windows investigation artifacts.

Some key findings included:

* **wevtutil** can be used by attackers to delete Windows Event Logs.
* **WDigest Registry Path**

  * `HKLM\SYSTEM\CurrentControlSet\Control\SecurityProviders\WDigest`
  * can be abused to expose credentials in memory.
* Host information such as hostname, OS version, and timezone helps establish investigation context.
* Suspicious user accounts and login sessions may indicate unauthorized access.
* Network connections reveal malicious executables and unusual ports.
* Registry startup entries, Userinit modifications, and DLL hijacking may indicate persistence.
* Services and Scheduled Tasks are common persistence mechanisms.
* Process trees help identify parent-child relationships.
* SHA256 hashes help verify suspicious executables.
* Temporary folders often contain malware, scripts, or attacker tools.

----------


# **Task 2 – Reliability of the System Tools**

## Question

**What tool did the adversary use to delete the logs?**

### Answer

**wevtutil**

### 🧠 AI SOC Explanation

`wevtutil` is a legitimate Windows command-line utility used to manage Event Logs. Attackers abuse it to **clear Windows Event Logs** and remove evidence after compromising a system. Clearing logs is a common **Defense Evasion** technique.

> **MITRE ATT&CK:** T1070.001 – Clear Windows Event Logs

---

## Question

**Which registry path was used to steal login credentials?**

### Answer

```text
HKLM:\SYSTEM\CurrentControlSet\Control\SecurityProviders\WDigest
```

### 🧠 Explanation

The **WDigest** registry key can store user credentials in memory when enabled. Attackers modify this registry setting to dump plaintext passwords using tools such as Mimikatz.

> **IOC:** Registry modification under WDigest.

---

# **Task 3 – System Profile**

## Question

**Hostname?**

### Answer

**CCTL-WS-018-B21**

### 🧠 Explanation

The hostname uniquely identifies the compromised computer. Analysts use it to correlate SIEM alerts, endpoint logs, and asset inventory.

---

## Question

**Operating System Version?**

### Answer

**10.0.17763**

### 🧠 Explanation

Knowing the Windows build helps determine applicable vulnerabilities, patches, and security configurations.

---

## Question

**Time Zone?**

### Answer

**Turkey Standard Time**

### 🧠 Explanation

The system timezone is important for building an accurate incident timeline and correlating events from multiple log sources.

---

# **Task 4 – Users and Sessions**

## Question

**How many suspicious accounts?**

### Answer

**3**

### 🧠 Explanation

Unexpected user accounts often indicate persistence or unauthorized access. Analysts verify whether the accounts are legitimate or attacker-created.

---

## Question

**Guest Account SID?**

### Answer

**S-1-5-21-1966530601-3185510712-10604624-501**

### 🧠 Explanation

A SID (Security Identifier) uniquely identifies each Windows account. Attackers sometimes rename built-in accounts, but the SID remains unchanged.

---

## Question

**Last login time of Admin account?**

### Answer

**2/28/2024 10:21:10 AM**

### 🧠 Explanation

Last logon timestamps help investigators determine when suspicious user activity occurred.

---

# **Task 5 – Network Scope**

## Question

**Malicious Process?**

### Answer

**INITIAL_LANTERN[.]exe**

### 🧠 Explanation

Unknown executables running from temporary folders often indicate malware or backdoors.

---

## Question

**Directory Path?**

### Answer

```text
C:\Users\Administrator\AppData\SpcTmp\
```

### 🧠 Explanation

Attackers frequently hide malware inside AppData or Temp folders because these locations receive less attention.

---

## Question

**Remote Port?**

### Answer

**8888**

### 🧠 Explanation

Port **8888** is commonly abused by malware, proxy tools, and Command-and-Control (C2) communications.

---

## Question

**Suspicious AnyDesk Path?**

### Answer

```text
D:\AnyDesk[.]exe
```

### 🧠 Explanation

Remote access tools like AnyDesk are legitimate but commonly abused by attackers for persistence and remote control.

---

## Question

**Firewall Rule Port?**

### Answer

**5985**

### 🧠 Explanation

Port **5985** is used by **Windows Remote Management (WinRM)**. Attackers frequently abuse WinRM for lateral movement.

> **MITRE:** T1021.006 – WinRM

---

# **Task 6 – Startup and Registry**

## Question

**User running AnyDesk?**

### Answer

**Public**

### 🧠 Explanation

Running applications through the Public profile may indicate persistence or attempts to avoid user-specific detection.

---

## Question

**Userinit Registry Value?**

### Answer

```text
C:\Windows\system32\userinit[.]exe, cmd[.]exe /c "start /min netsh[.]exe -c"
```

### 🧠 Explanation

The **Userinit** registry key runs during user logon. Attackers modify it to launch malicious commands automatically whenever a user signs in.

---

## Question

**Suspicious DLL?**

### Answer

```text
.\fwshield.dll
```

### 🧠 Explanation

Malicious DLLs may be loaded through registry hijacking to execute attacker-controlled code.

---

# **Task 7 – Services and Scheduled Items**

## Question

**Suspicious Service?**

### Answer

**LMVCSS**

### 🧠 Explanation

Attackers create fake Windows services to maintain persistence after reboot.

---

## Question

**SHA256 Hash?**

### Answer

**E9AA7564B2D1D612479E193A9F8CB70DF9CFBE02A39900EEE22FE266F5320EBF**

### 🧠 Explanation

SHA256 hashes uniquely identify files. Analysts compare hashes against VirusTotal or internal threat intelligence.

---

## Question

**Suspicious Non-running Service?**

### Answer

**aurora-agent**

### 🧠 Explanation

Even inactive services may indicate malware installed for future execution.

---

## Question

**Original Filename?**

### Answer

```text
x3xv5weg[.]exe
```

### 🧠 Explanation

Random filenames often indicate malware attempting to hide its identity.

---

# **Task 8 – Processes and Directories**

## Question

**Parent Process of INITIAL_LANTERN?**

### Answer

```text
services[.]exe
```

### 🧠 Explanation

The parent process reveals how malware was launched. Analysts investigate unusual parent-child process relationships.

---

## Question

**SSH Username?**

### Answer

**James**

### 🧠 Explanation

Identifying targeted usernames helps determine whether attackers attempted brute-force or credential attacks.

---

## Question

**Parent Process of Aurora?**

### Answer

```text
svchost[.]exe
```

### 🧠 Explanation

Attackers often inject malicious code into legitimate Windows processes such as `svchost.exe`.

---

## Question

**Executable in Temp Folder?**

### Answer

```text
jmp[.]exe
```

### 🧠 Explanation

Executables inside Temp directories are suspicious because attackers commonly execute malware from temporary locations.

---

## Question

**Proxy Script?**

### Answer

```text
Invoke-SocksProxy[.]psm1
```

### 🧠 Explanation

PowerShell proxy scripts allow attackers to tunnel traffic and bypass network restrictions.

---

## Question

**SHA256 of Proxy Script?**

### Answer

**E7697645F36DE5978C1B640B6B3FC819E55B00EE8D9E9798919C11CC7A6FC88B**

### 🧠 Explanation

File hashes help analysts identify known malware families and verify file integrity.

---

## Question

**Hidden Disk Label?**

### Answer

**Setups**

### 🧠 Explanation

Hidden partitions or volumes are frequently used to hide attacker tools, malware, or stolen data.

---

# **Task 9 – Conclusion**

### 🧠 AI SOC  Summary

This investigation demonstrates a complete Windows compromise involving **log deletion (`wevtutil`)**, **credential theft (WDigest)**, **malicious services**, **registry persistence**, **AnyDesk abuse**, **PowerShell proxy scripts**, **suspicious processes**, **WinRM activity**, and **hidden storage**. As a SOC Analyst, the investigation is not about finding a single malicious file—it is about correlating evidence across logs, registry keys, services, processes, network connections, and file hashes to build a complete attack timeline and determine the attacker's persistence and objectives.

---

## 🛡 IOC Examples

* `INITIAL_LANTERN[.]exe`
* `Invoke-SocksProxy[.]psm1`
* `fwshield.dll`
* `aurora-agent`
* `AnyDesk[.]exe`
* Port **8888**
* WinRM Port **5985**
* WDigest Registry Changes
* Cleared Windows Event Logs
* Unknown Windows Services

---

## 🎯 MITRE ATT&CK Techniques

* **T1070.001** – Clear Windows Event Logs
* **T1003** – OS Credential Dumping
* **T1112** – Modify Registry
* **T1543.003** – Create or Modify Windows Service
* **T1059.001** – PowerShell
* **T1021.006** – Windows Remote Management (WinRM)
* **T1078** – Valid Accounts
* **T1572** – Protocol Tunneling

------

## How I improved?

Before this room, I mainly looked at malware as a single malicious file.

Now I understand that an investigation requires collecting evidence from many Windows artifacts and connecting them together to explain:

* Initial access
* Execution
* Persistence
* Credential access
* Command execution
* Network communication

Instead of focusing on one file, I now think about the complete attack chain.

## Real-world relevance

In a real SOC, analysts rarely receive complete information.

Instead, they investigate alerts by examining:

* Event Logs
* Registry changes
* Running processes
* Services
* Scheduled Tasks
* Startup entries
* Network activity
* User sessions

These artifacts help determine whether an alert represents normal administrative activity or an actual security incident.

---

# 🛡️ SOC Analyst L1: Mindset 

## Tricky Connection

A beginner often asks:

> "What malware infected this computer?"

A SOC Analyst asks:

> "What evidence did the attacker leave behind, and what does each artifact tell me?"

This room shifted my mindset from searching for malware to reconstructing attacker behavior using forensic evidence.

### Room Questions and Investigation Highlights

| Investigation Area | Key Finding                                                |
| ------------------ | ---------------------------------------------------------- |
| Log Tampering      | `wevtutil` used to delete logs                             |
| Credential Access  | WDigest Registry path modified                             |
| Host Profile       | Hostname, OS version, Time Zone identified                 |
| User Investigation | 3 suspicious accounts discovered                           |
| Network Activity   | INITIAL_LANTERN[.]exe communicating over port 8888         |
| Persistence        | Userinit modified, LMVCSS service, AnyDesk startup         |
| Malware Evidence   | aurora-agent service, fwshield.dll, Invoke-SocksProxy.psm1 |
| Process Analysis   | Parent-child process relationships investigated            |
| File Verification  | SHA256 hashes collected for suspicious files               |

### What should I learn next?

To strengthen this knowledge, I plan to study:

* Windows Event IDs
* Sysmon Logs
* Windows Registry Internals
* Process Creation (Event ID 4688)
* PowerShell Logging
* Persistence Techniques (MITRE ATT&CK)
* Timeline Analysis
* Microsoft Defender XDR
* Microsoft Sentinel

---

# 💼 Scenario-Based  Question (Critical Thinking)

## Question

A Microsoft Defender alert reports suspicious PowerShell activity on a Windows workstation. During your investigation, you discover:

* Windows Event Logs have been cleared.
* A new service named **LMVCSS** exists.
* The **Userinit** Registry value has been modified.
* A suspicious executable is communicating over port **8888**.

As a SOC Analyst L1, how would you approach this investigation?

## Answer Concept

I would begin by preserving evidence and reviewing all available endpoint telemetry. I would examine the process tree to identify the parent process, verify the SHA256 hash of the executable, review Registry modifications, inspect services and scheduled tasks for persistence, and analyze network connections associated with port 8888.

I would also determine whether the cleared event logs indicate anti-forensic activity, correlate findings with other security tools, and document a complete timeline before escalating the incident. My focus would be on understanding the attacker's behavior rather than relying on a single indicator.

---
