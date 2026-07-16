#  Walkthrough: [Windows Threat Detection 1](https://tryhackme.com/room/windowsthreatdetection1)
<img width="601" height="600" alt="image" src="https://github.com/user-attachments/assets/ef0bd68c-1db6-4819-b266-6c432ef8fb8c" />


As part of my **SOC Analyst L1** learning journey, I completed the **Windows Threat Detection 1** room to understand how attackers gain **Initial Access** to Windows systems and how defenders detect these activities using Windows Event Logs and Sysmon.

Before this room, my biggest confusion was:

> **"How does a SOC Analyst detect an attack when they were not present during the compromise?"**

This room answered that question by showing that attackers always leave traces in logs. A SOC Analyst's job is to collect those traces, investigate them, and determine whether the activity is normal or malicious.

---

### Who is this for?

* SOC Analysts (L1/L2)
* Incident Responders
* Blue Team Members
* Windows Administrators
* Anyone learning Threat Detection

### What is it?

This room introduces **Windows Initial Access detection**. It focuses on identifying how attackers first compromise Windows systems through techniques such as RDP brute force, phishing, and infected USB devices.

### Where does this apply?

* Enterprise Windows environments
* Security Operations Centers (SOC)
* Active Directory networks
* Incident Response investigations

### When should I learn this?

After learning Windows Fundamentals, Networking Basics, and Event Logs, but before studying advanced incident response or threat hunting.

### Why is it important?

Every cyberattack begins with **Initial Access**. Detecting attackers early reduces the impact of ransomware, malware infections, and account compromise.

### How does it work?

SOC Analysts investigate:

* Windows Security Logs (Event IDs **4624** and **4625**)
* Sysmon Process Creation Events
* Parent-child process relationships
* Browser downloads
* Network connections
* File execution history

Each Initial Access technique leaves different evidence that analysts can correlate during an investigation.

---

## What confused me?

Initially, I thought detecting attacks meant simply running antivirus software.

During this room, I realized detection is based on **evidence** rather than assumptions.

Some questions I had were:

* Why is Event ID **4625** so important?
* How can analysts identify phishing if the email has already been opened?
* Why are **.LNK**, **.COM**, and **double-extension (.jpg.exe)** files dangerous?
* How do analysts know malware came from a USB drive?

Learning how Windows logs record user actions helped answer these questions.

---

## What I learned?

This room introduced several common Initial Access techniques:

### 1. RDP Brute Force

* Multiple failed logins (Event ID **4625**)
* Successful login (Event ID **4624**, Logon Type 10)
* Attacker IP Address
* Workstation Name

Example:

* Threat Actor IP:
  `203.205.34.107`

* Workstation:
  `DESKTOP-QNBC4UU`

---

### 2. Phishing

I learned how attackers disguise malware using:

* `.COM` files
* `.LNK` shortcuts
* Double extensions (`best-cat.jpg.exe`)

The room also demonstrated:

* Downloaded ZIP archives
* Malware execution
* Process IDs
* Malicious domains such as:

`rjj.store`

---

### 3. USB Malware

I learned that malware can:

* Execute automatically from USB
* Drop payloads onto disk
* Spread itself to additional removable drives

Example:

```
E:\Open Sandisk 4GB USB.exe
```

Dropped file:

```
C:\Users\Public\Documents\winupdate.exe
```

---

## How I improved?

Before this room, I only understood attacks from an attacker's perspective.

Now I think more like a defender.

Instead of asking:

> "How does malware infect Windows?"

I now ask:

> "What evidence does malware leave behind, and where can I find it?"

That small mindset shift is exactly what a SOC Analyst needs.

---

## Real-world relevance

Inside an enterprise SOC:

* Brute-force attempts generate authentication alerts.
* Suspicious downloads are monitored using Endpoint Detection and Response (EDR).
* USB activity may violate company policy.
* Phishing campaigns are investigated before they spread.

The techniques covered in this room represent common attack methods that SOC teams investigate daily.

---

# 🛡️ SOC Analyst L1: Mindset & Tricky Connection

## Tricky Connection

An attacker thinks:

> "How do I gain access?"

A SOC Analyst thinks:

> "How would I detect someone gaining access?"

That difference is the foundation of Blue Team thinking.

For example:

* Red Team performs an RDP brute-force attack.
* Blue Team monitors Event IDs **4624** and **4625**.
* Red Team sends phishing emails.
* Blue Team investigates unusual process execution, downloaded files, and network connections.
* Red Team uses USB malware.
* Blue Team reviews removable device activity and newly created files.

This room showed me that understanding attacker techniques is valuable because it helps defenders build better detections.

### Next Learning Steps

To strengthen this knowledge, I plan to study:

* Windows Event Logs
* Sysmon
* MITRE ATT&CK Framework
* Windows Process Creation (Event ID 4688)
* PowerShell Logging (Event ID 4104)
* Microsoft Defender for Endpoint
* Microsoft Sentinel SIEM

---

# 💼 Scenario-Based  Question (Critical Thinking)

### Question

Your SIEM generates an alert showing:

* Over 150 failed RDP login attempts (Event ID 4625)
* One successful RDP login (Event ID 4624, Logon Type 10)
* Source IP: **203.205.34.107**

What would you do as a SOC Analyst L1?

### Answer

My investigation would follow these steps:

1. Verify whether the successful login belongs to a legitimate user.
2. Review authentication logs to confirm the brute-force pattern.
3. Check the source IP for reputation and previous activity.
4. Review process creation events after the login for suspicious execution.
5. Look for persistence mechanisms, privilege escalation, or unusual network connections.
6. Collect evidence and escalate the incident according to the organization's incident response process.

I would avoid making assumptions and base my findings on log evidence before determining whether the activity represents an account compromise.

---


# **Task 1 – Introduction**

### 🎯 Objective

Learn how attackers gain their **first access** to a target system. Initial Access is the first stage of the Cyber Kill Chain and MITRE ATT&CK. Detecting this stage early allows SOC analysts to stop attacks before privilege escalation, lateral movement, or ransomware deployment.

---

# **Task 2 – Intro to Initial Access**

## Question

**Which MITRE ATT&CK technique ID describes Initial Access via a vulnerable mail server?**

### Answer

**T1190**

### 🧠 AI SOC Professor Explanation

**T1190 – Exploit Public-Facing Application** occurs when attackers exploit vulnerabilities in internet-facing applications such as mail servers, VPN gateways, or web applications. Once exploited, attackers gain initial access without needing valid credentials.

> **SOC Detection:** Monitor web server logs, IDS alerts, WAF logs, and unusual HTTP requests targeting vulnerable services.

---

## Question

**Which Initial Access method relies on a user opening a malicious email attachment?**

### Answer

**Phishing**

### 🧠 Explanation

Phishing tricks users into opening malicious attachments or clicking harmful links. The malware executes only after user interaction, making user awareness and email security important defensive controls.

> **IOC Examples**

* Suspicious email attachments
* Double-extension files
* Office documents with macros
* Unexpected ZIP files

> **MITRE ATT&CK**

* **T1566 – Phishing**

---

# **Task 3 – Initial Access via RDP**

## Question

**Which user seems to be most actively brute-forced?**

### Answer

**Administrator**

### 🧠 Explanation

Attackers commonly target the **Administrator** account because it usually has the highest privileges. Repeated failed logins against this account often indicate brute-force attempts.

---

## Question

**Which IP successfully logged in through RDP (Logon Type 10)?**

### Answer

**203.205.34.107**

### 🧠 Explanation

**Logon Type 10** indicates a successful Remote Desktop Protocol (RDP) login. Analysts should verify whether the source IP is trusted or suspicious.

> **Windows Event ID**

* **4624** (Successful Login)
* **4625** (Failed Login)

---

## Question

**What was the attacker's workstation name?**

### Answer

**DESKTOP-QNBC4UU**

### 🧠 Explanation

The workstation name identifies the system initiating the connection. This information helps investigators correlate multiple attacks originating from the same host.

---

# **Task 4 – Initial Access via Phishing**

## Question

**What flag appeared after executing the COM file?**

### Answer

**THM{misleading_extension}**

### 🧠 Explanation

The attacker disguised malware using a misleading file extension. Many users mistake `.com` files for harmless documents, but they are executable programs.

---

## Question

**Which URL downloaded the second-stage malware?**

### Answer

**[http://wp16.hqywlqpa.thm:8000/cgi-bin/f](http://wp16.hqywlqpa.thm:8000/cgi-bin/f)**

### 🧠 Explanation

The first malware acts as a **downloader**, contacting a remote server to retrieve additional malicious payloads.

> **IOC**

* Suspicious URLs
* Unknown domains
* HTTP downloads from uncommon ports

---

## Question

**Which double-extension file was found?**

### Answer

**best-cat.jpg.exe**

### 🧠 Explanation

Attackers often use **double extensions** to trick users into believing an executable is an image or document.

> **SOC Detection**
> Alert on:

* `.jpg.exe`
* `.pdf.exe`
* `.doc.exe`

---

# **Task 5 – Continuing Phishing**

## Question

**Which ZIP file was downloaded?**

### Answer

**C:\Users\Administrator\Downloads\top-cats.zip**

### 🧠 Explanation

Compressed files are commonly used to bypass email filtering and hide malware.

---

## Question

**Where was the ZIP extracted?**

### Answer

**C:\Users\Administrator\Pictures**

### 🧠 Explanation

Attackers often encourage users to extract malicious files into common folders before execution.

---

## Question

**What was the malware Process ID?**

### Answer

**5484**

### 🧠 Explanation

The Process ID (PID) uniquely identifies the running malware. Analysts use it to investigate child processes, network activity, and persistence.

---

## Question

**Which malicious domain did the malware contact?**

### Answer

**rjj.store**

### 🧠 Explanation

This domain likely acted as a **Command and Control (C2)** server. SOC analysts should block the domain and investigate any communication with it.

> **IOC**

* Domain: `rjj.store`

---

# **Task 6 – Initial Access via USB**

## Question

**Which USB file did the user execute?**

### Answer

**E:\Open Sandisk 4GB USB.exe**

### 🧠 Explanation

USB malware often disguises itself as a legitimate drive shortcut or installer to trick users into executing it.

---

## Question

**Which file did the malware drop?**

### Answer

**C:\Users\Public\Documents\winupdate.exe**

### 🧠 Explanation

After execution, malware commonly drops another executable onto disk to maintain persistence or launch additional payloads.

> **IOC**
> `winupdate.exe`

---

## Question

**Which USB drive was infected next?**

### Answer

**F:**

### 🧠 Explanation

The malware propagated to another USB drive, demonstrating **worm-like behavior** that enables it to spread between systems.

---

# **Task 7 – Conclusion**

### 🧠  Summary

This room demonstrated the three most common Windows Initial Access techniques:

* **RDP attacks**
* **Phishing attacks**
* **USB malware**

As a SOC analyst, detecting these attacks early significantly reduces the impact of a compromise. Monitoring authentication logs, process creation, email attachments, USB activity, and outbound connections enables rapid incident detection and response.

---

# 🛡 IOC Examples

* Multiple failed RDP logins
* Event IDs **4624** and **4625**
* Double-extension files (`.jpg.exe`)
* `.com` executable attachments
* Suspicious ZIP archives
* Unknown domains (`rjj.store`)
* `winupdate.exe`
* USB executable files
* Unexpected outbound HTTP requests

---

# 🎯 MITRE ATT&CK Mapping

| Technique                         | ATT&CK ID     |
| --------------------------------- | ------------- |
| Exploit Public-Facing Application | **T1190**     |
| Phishing                          | **T1566**     |
| Remote Services (RDP)             | **T1021.001** |
| User Execution                    | **T1204**     |
| Command and Scripting Interpreter | **T1059**     |

---

# 🔍 SOC Detection Opportunities

* Detect repeated failed RDP logins (Event ID **4625**).
* Monitor successful RDP logins from unfamiliar IPs (Event ID **4624**, Logon Type **10**).
* Alert on execution of `.com` and double-extension files.
* Detect execution from **Downloads**, **Desktop**, or **USB drives**.
* Monitor outbound connections to newly observed or suspicious domains.
* Identify process chains where Office apps, Explorer, or shortcuts (`.lnk`) launch PowerShell or command interpreters.

> **AI SOC Professor Tip:** Initial Access is where defenders have the best chance to stop an attack. If you can recognize brute-force attempts, phishing execution, or malicious USB activity at this stage, you may prevent credential theft, ransomware deployment, lateral movement, and data exfiltration before they occur.


---

## Final Reflection

This room helped me understand that **Initial Access is one of the most important stages of an attack** because every compromise starts somewhere. Rather than focusing on how attackers break in, I learned how to identify their actions through Windows logs, process creation events, and network evidence. This is an essential mindset for becoming a SOC Analyst L1, where decisions should always be based on observable evidence instead of assumptions.

