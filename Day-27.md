# Walkthrough: [Windows Logging for SOC](https://tryhackme.com/room/windowsloggingforsoc?vccr=1)

<img width="600" height="600" alt="image" src="https://github.com/user-attachments/assets/a9e217fe-f516-4f48-b053-21d8efe8fff6" />

As part of my SOC Analyst L1 learning journey, this room introduced me to one of the most important skills in Blue Team operations: **Windows Log Analysis**.

Before this room, I knew Windows generated logs, but I wasn't sure **which logs were useful**, **which Event IDs mattered**, or **how SOC analysts connect multiple log sources to investigate an attack**.

The objective of this room was to understand how attackers leave evidence behind in Windows Event Logs, Sysmon logs, and PowerShell history, and how these logs can be correlated to reconstruct an entire attack.

---

### Who is this for?

* SOC Analysts
* Blue Team members
* Incident Responders
* Windows Administrators
* Digital Forensics investigators

### What is Windows Logging?

Windows Logging is the process of recording system, user, application, and security activities performed on a Windows computer.

These logs provide evidence of what happened on a system and help defenders investigate suspicious activity.

### Where does this apply?

* Enterprise Windows environments
* Security Operations Centers (SOC)
* Active Directory infrastructures
* Incident Response investigations
* Digital Forensics

### When should it be learned?

After understanding:

* Windows Fundamentals
* Networking Basics
* Authentication
* Cyber Kill Chain
* SIEM Fundamentals

### Why is it important?

Attackers cannot perform actions without leaving traces.

Windows logs help analysts answer questions such as:

* Who logged in?
* When did they log in?
* From where?
* What processes were executed?
* Which files were created?
* Which network connections were established?

### How does it work?

Windows continuously records events into different log sources such as:

* Security Logs
* System Logs
* Application Logs
* Sysmon Logs
* PowerShell Logs

SOC analysts correlate these logs to reconstruct an attack timeline.

---

### What confused me?

Initially, I found it difficult to understand:

* Why Windows has multiple log sources.
* The difference between Security Logs and Sysmon.
* Why Event IDs like **4624** and **4625** are considered so important.
* How Logon ID and Process ID help correlate events.

At first, the logs looked like thousands of unrelated entries.

### What I learned?

This room taught me that every attacker action leaves digital evidence.

I learned how to investigate:

* Successful logins
* Failed login attempts
* User creation
* Privilege escalation
* Process execution
* Malware downloads
* Network connections
* Persistence mechanisms
* PowerShell activity

Instead of viewing logs individually, I learned to correlate them using **Logon IDs**, **Process IDs**, timestamps, and related events.

### How I improved?

Before this room, I mainly focused on alerts.

Now I understand that logs tell the complete story behind those alerts.

Instead of asking:

> "Why did the alert trigger?"

I now ask:

> "What happened before and after this event?"

This mindset is essential for incident investigation.

### Real-World Relevance

Windows Event Logs are among the primary data sources ingested into SIEM platforms such as Microsoft Sentinel, Splunk, QRadar, and Microsoft Defender XDR.

Every day, SOC analysts investigate:

* Brute-force attacks
* RDP logins
* Privilege escalation
* Malware execution
* Persistence techniques
* PowerShell abuse

This room directly reflects real enterprise investigations.

---

# **Task 1 – Introduction**

Learn how SOC analysts investigate cyber incidents using **Windows Security Logs, Sysmon logs, and PowerShell logs**. Instead of looking at a single alert, analysts correlate multiple log sources to reconstruct the attack timeline and identify attacker behavior.

---

# **Task 2 – What Is Logged**

## **Question**

**Which Event ID describes a successful login?**

### **Answer**

**Security / 4624**

### 🧠 Explanation

**Event ID 4624** records every successful user authentication on a Windows system. It includes details such as the username, source IP, logon type, Logon ID, and authentication method. SOC analysts use this event to verify who logged in, from where, and whether the activity is legitimate.

> **SOC Note:** Correlate **4624 (Success)** with **4625 (Failed Login)** to detect brute-force attacks or credential stuffing.

---

# **Task 3 – Security Log: Authentication**

## **Question**

**Which IP performed the brute-force attack?**

### **Answer**

**10.10.53.248**

### 🧠 Explanation

Multiple failed login attempts from the same IP usually indicate a **brute-force attack**. SOC analysts investigate the source IP, login frequency, and target accounts to determine if the activity is malicious.

---

## **Question**

**Which user account was compromised?**

### **Answer**

**Administrator**

### 🧠 Explanation

The attacker successfully authenticated using the **Administrator** account. Privileged accounts are high-value targets because they provide elevated system access.

> **Detection Tip:** Alert on successful logins to privileged accounts after multiple failed attempts.

---

## **Question**

**What was the malicious Logon ID?**

### **Answer**

**0x183C36D**

### 🧠 Explanation

A **Logon ID** uniquely identifies a login session. Analysts use it to correlate related events—such as process creation, privilege changes, and user management—belonging to the same attacker session.

---

# **Task 4 – Security Log: User Management**

## **Question**

**Which user did the attacker create?**

### **Answer**

**svc_sysrestore**

### 🧠 Explanation

Attackers often create **backdoor accounts** to maintain persistence after gaining access. New administrative users should always be investigated.

---

## **Question**

**Which privileged groups was the account added to?**

### **Answer**

**Backup Operators, Remote Desktop Users**

### 🧠 Explanation

Adding a user to privileged groups increases the attacker's capabilities. **Backup Operators** can access sensitive files, while **Remote Desktop Users** allows remote access to the machine.

> **SOC Detection:** Monitor **Event IDs 4720 (User Created)** and **4732/4733 (Group Membership Changes).**

---

## **Question**

**Did the Logon ID match the previous task?**

### **Answer**

**Yea**

### 🧠 Explanation

Matching Logon IDs confirm that the same login session performed multiple malicious actions. Correlating events by Logon ID helps reconstruct the attack chain.

---

# **Task 5 – Sysmon: Process Monitoring**

## **Question**

**Which browser did Sarah use?**

### **Answer**

**Google Chrome**

### 🧠 Explanation

Sysmon records process creation events, allowing analysts to identify which applications users executed before an incident.

---

## **Question**

**Which file was downloaded?**

### **Answer**

```text
C:\Users\sarah.miller\Downloads\ckjg.exe
```

### 🧠 Explanation

Downloaded executable files are common malware delivery methods. Analysts verify the file's hash, source, and behavior to determine whether it is malicious.

---

## **Question**

**Where was the file downloaded from?**

### **Answer**

```text
http://gettsveriff.com/bgj3/ckjg.exe
```

### 🧠 Explanation

The download URL identifies the malware source. Analysts check the domain and URL against threat intelligence platforms to assess reputation and identify related campaigns.

---

# **Task 6 – Sysmon: Files and Network**

## **Question**

**Which persistence file was created?**

### **Answer**

```text
C:\Users\sarah.miller\AppData\Roaming\Microsoft\Windows\Start Menu\Programs\Startup\DeleteApp.url
```

### 🧠 Explanation

The malware placed a file in the **Startup** folder, ensuring it runs automatically whenever the user logs in. This is a common persistence technique.

---

## **Question**

**What was the Command & Control (C2) server?**

### **Answer**

**193.46.217.4:7777**

### 🧠 Explanation

A **C2 server** allows attackers to remotely control infected systems. Outbound connections to unknown IPs or uncommon ports (such as **7777**) should be investigated immediately.

---

## **Question**

**Which domain was associated with the malicious IP?**

### **Answer**

**hkfasfsafg.click**

### 🧠 Explanation

Attackers often use newly registered or suspicious domains for malware communication. SOC analysts correlate domains with DNS logs and threat intelligence feeds.

---

# **Task 7 – PowerShell Logging**

## **Question**

**Which PowerShell command was executed first?**

### **Answer**

```powershell
Get-ComputerInfo
```

### 🧠 Explanation

Attackers frequently begin by gathering system information. `Get-ComputerInfo` reveals details such as the operating system, hardware, installed updates, and system configuration.

---

## **Question**

**When was the first PowerShell command executed?**

### **Answer**

**May 18, 2025**

### 🧠 Explanation

Timestamps help analysts build an accurate incident timeline and correlate PowerShell activity with other events such as logins and process creation.

---

## **Question**

**What flag was stored in the PowerShell history?**

### **Answer**

```text
THM{it_was_me!}
```

### 🧠 Explanation

PowerShell history records previously executed commands. During real investigations, analysts review it to identify malicious scripts, encoded commands, downloads, and attacker actions.

---

# **Task 8 – Conclusion**

### 🧠  Summary

This room demonstrated how to investigate a complete attack using **Windows Security Logs, Sysmon, and PowerShell history**. By correlating authentication events (4624/4625), Logon IDs, process creation, user management, file creation, network connections, and PowerShell activity, SOC analysts can reconstruct the attack timeline from initial access to persistence and command-and-control communication.

---

# 🛡 IOC (Indicators of Compromise)

* Multiple **4625** failed logins followed by **4624** success
* Successful RDP login (**Logon Type 10**) from an unknown IP
* Creation of a new administrative user (`svc_sysrestore`)
* Addition to privileged groups
* Download of suspicious executable (`ckjg.exe`)
* Startup folder persistence (`DeleteApp.url`)
* Outbound connection to `193.46.217.4:7777`
* Communication with suspicious domain `hkfasfsafg.click`
* Suspicious PowerShell commands in history

---

# 🎯 MITRE ATT&CK Mapping

| Technique                  | ATT&CK ID     |
| -------------------------- | ------------- |
| Brute Force                | **T1110**     |
| Valid Accounts             | **T1078**     |
| Remote Services (RDP)      | **T1021.001** |
| Account Manipulation       | **T1098**     |
| User Execution             | **T1204**     |
| PowerShell                 | **T1059.001** |
| Startup Folder Persistence | **T1547.001** |
| Command and Control        | **T1071**     |

---

# 🔍 SOC Detection Opportunities

* Alert on repeated **4625** failed logins followed by **4624**.
* Monitor **Logon Type 10 (RDP)** from unusual IP addresses.
* Detect new user account creation and privileged group changes.
* Monitor Sysmon **Event ID 1 (Process Creation)** for suspicious executables.
* Alert on outbound connections to unknown IPs or high-risk ports.
* Enable and review **PowerShell Script Block Logging (Event ID 4104)**.
* Correlate **Logon ID**, **Process ID**, and **Sysmon events** to reconstruct the full attack chain.

> **AI SOC  Tip:** Never investigate a Windows alert in isolation. The real strength of a SOC analyst lies in **correlation**—linking Security logs, Sysmon events, PowerShell history, and network activity to answer four key questions: **Who** performed the action, **What** they did, **When** it happened, and **How** they maintained access. This end-to-end view is what turns raw logs into a complete incident investigation.

---

# 🛡️ SOC Analyst L1: Mindset

### Tricky Connection

A beginner sees:

* Event ID 4625
* Sysmon Event
* PowerShell command

A SOC Analyst sees:

* Initial Access
* Credential Attack
* Malware Execution
* Persistence
* Command & Control

The logs themselves are only pieces of evidence.

The real skill is **correlating them into one timeline**.

### Next Learning Steps

To strengthen this knowledge, I should continue learning:

1. Windows Event IDs (4624, 4625, 4688, 4720, 4728, 7045)
2. Sysmon Event IDs
3. Windows Registry Forensics
4. PowerShell Logging
5. Microsoft Sentinel (KQL)
6. Splunk Log Analysis
7. MITRE ATT&CK Mapping
8. Sigma Detection Rules
9. Incident Timeline Reconstruction

---

# 💼 Scenario-Based Question (Critical Thinking)

### Scenario

While monitoring Microsoft Sentinel, you observe:

* Hundreds of Event ID **4625** (Failed Logons) from a single external IP.
* Shortly afterward, an Event ID **4624** (Successful Logon) using the **Administrator** account.
* Minutes later, Event ID **4720** indicates a new user account has been created.

### Question

How would you investigate this incident as an L1 SOC Analyst?

### Answer Concept

1. **Validate the Alert**

   * Confirm repeated Event ID 4625 entries followed by Event ID 4624.
   * Verify the source IP, username, and timestamps.

2. **Correlate Events**

   * Match the events using the **Logon ID**.
   * Determine whether the successful login belongs to the same session as the failed attempts.

3. **Investigate Post-Login Activity**

   * Review Event ID 4720 to identify the newly created account.
   * Check whether it was added to privileged groups.
   * Review Sysmon logs for suspicious processes and network connections.

4. **Containment**

   * Disable the compromised account.
   * Disable the attacker-created account.
   * Isolate the affected endpoint if malicious activity is confirmed.

5. **Escalation**

   * Escalate the incident to the L2 SOC or Incident Response team with a clear timeline, affected systems, evidence, and recommended actions.

This structured investigation demonstrates log correlation, attack reconstruction, and incident handling—core skills expected from an entry-level SOC Analyst.

---

#  Final Reflection

This room changed how I view Windows logs. Instead of treating Event IDs as isolated entries, I learned to connect Security Logs, Sysmon, and PowerShell history into a single attack timeline.

My biggest takeaway is that **logs are evidence**, and the role of a SOC Analyst is to piece together that evidence to understand **who**, **what**, **when**, **where**, **how**, and **why** an attack occurred. Developing this investigative mindset is a major step toward becoming an effective SOC Analyst L1.

