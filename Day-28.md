
#  Walkthrough: [Compromised Windows Analysis](https://tryhackme.com/room/compromisedwindowsanalysis)

<img width="1200" height="1200" alt="image" src="https://github.com/user-attachments/assets/cfb7903d-9eec-40df-a1f7-510293e4d154" />


As part of my SOC Analyst L1 learning journey, this room introduced me to Windows Digital Forensics and Incident Response (DFIR). The objective was to investigate a compromised Windows system, identify how the attacker gained access, determine what actions they performed, and reconstruct the complete attack timeline using Windows forensic artifacts.

My biggest question before starting was:

> **How can a SOC Analyst investigate an attack after the attacker has already deleted the malicious files?**

This room showed me that Windows stores many forensic artifacts that help investigators reconstruct attacker activity even after files are removed.

---


### Who is this for?

* SOC Analysts
* Digital Forensics Analysts
* Incident Responders
* Threat Hunters

### What is it?

A Windows forensic investigation where multiple artifacts are analyzed to identify attacker behavior and reconstruct an incident timeline.

### Where does this apply?

* Enterprise Windows environments
* SOC investigations
* Incident response engagements
* Malware investigations

### When should I learn this?

After understanding Windows fundamentals and before learning advanced incident response.

### Why is it important?

Attackers often delete malware to hide their tracks, but Windows leaves behind forensic evidence that investigators can use.

### How does it work?

Different Windows artifacts record different activities.

* Event Logs record system activity.
* Prefetch records program execution.
* Amcache records executed applications.
* LNK files reveal recently accessed files.
* Scheduled Tasks reveal persistence mechanisms.

Combining these artifacts creates a complete picture of the attack.

---



### What confused me?

Initially, I believed that deleting malware would remove all evidence of an attack.

I was also confused about:

* Why investigators use multiple forensic artifacts.
* Why the same activity appears in different Windows locations.
* How analysts determine the order of attacker actions.

After completing the room, I understood that every artifact provides only part of the story, and combining them produces the complete timeline.

### What I learned?

This room introduced me to several important forensic artifacts.

I learned how to investigate:

* Scheduled Tasks for persistence
* LNK files for recently opened files
* Prefetch for executed programs
* Amcache for executable metadata
* Windows Event Logs for authentication and Defender events

I also learned how attackers disable security controls before executing malware.

### How I improved?

Before this room, I focused mainly on malware itself.

Now I understand that successful investigations rely on evidence collected from operating system artifacts rather than assumptions.

Instead of asking,

> "Where is the malware?"

I now ask,

> "What evidence did the attacker leave behind?"

-----------

# **Task 1 – Introduction**

## Question

**What is the user's name whose system generated suspicious SSH traffic to a malicious IP?**

### Answer

**Aashir**

### 🧠 AI SOC  Explanation

The compromised user's name is **Aashir**. His computer repeatedly attempted SSH connections to a malicious IP every minute without his knowledge. This behavior indicates **malware persistence**, where malicious software automatically runs at scheduled intervals. Windows Defender was also disabled, suggesting the attacker tried to avoid detection before maintaining remote access.

> **SOC Note:** Unexpected outbound SSH traffic from a Windows workstation is highly suspicious.

---

# **Task 2 – Lab Setup**

## Question

**You are good to go!**

### Answer

**Lab Ready**

### 🧠 Explanation

This task prepares the investigation environment by providing forensic tools and evidence. Before analyzing any incident, SOC analysts verify that all required tools and evidence are available.

---

# **Task 3 – Timeline Explorer**

## Question

**Which tool makes CSV analysis easier?**

### Answer

**Timeline Explorer**

### 🧠 Explanation

Timeline Explorer displays forensic CSV files in a readable timeline. Analysts use it to correlate events from different artifacts and reconstruct attacker activity in chronological order.

> **SOC Tip:** Building a timeline is one of the first steps during incident response.

---

# **Task 4 – Investigating Persistence**

## Question

**What is the scheduled task created by the attacker?**

### Answer

**CnC**

### 🧠 Explanation

The attacker created a scheduled task named **CnC** to maintain persistence. It automatically executed every minute, allowing the malware to repeatedly contact the Command-and-Control (C2) server.

---

## Question

**What is the malicious IP?**

### Answer

**101.55.125.10**

### 🧠 Explanation

This IP address is the external server receiving repeated SSH connections. SOC analysts immediately investigate such IPs using threat intelligence platforms and block them if confirmed malicious.

> **IOC:** `101.55.125.10`

---

# **Task 5 – Recently Accessed Files (LNK Analysis)**

## Question

**What RAR file was created?**

### Answer

**Cursed.rar**

### 🧠 Explanation

The attacker delivered malware inside **Cursed.rar**. Compressed archives are commonly used to bypass email filters and antivirus detection.

---

## Question

**When was it created?**

### Answer

**2025-03-29 10:26:07**

### 🧠 Explanation

Creation timestamps help analysts determine exactly when the attacker introduced malicious files into the system and correlate them with other events.

---

# **Task 6 – File Execution (Prefetch Analysis)**

## Question

**What malicious executable was launched?**

### Answer

**Cipher.exe**

### 🧠 Explanation

Prefetch confirms that **Cipher.exe** executed on the system. Prefetch artifacts help investigators identify programs that actually ran, even if the files were later deleted.

---

## Question

**How many times was it executed?**

### Answer

**2**

### 🧠 Explanation

The malware executed twice, indicating either repeated manual execution or execution by persistence mechanisms.

---

## Question

**Last execution time?**

### Answer

**2025-03-29 10:29:12**

### 🧠 Explanation

Execution timestamps allow analysts to place malware activity accurately within the attack timeline.

---

# **Task 7 – Executable Investigation (Amcache)**

## Question

**Full path of the malicious executable?**

### Answer

```text
C:\Users\Administrator\Desktop\Cursed\Cipher.exe
```

### 🧠 Explanation

Amcache records executed applications and their original locations. This helps investigators identify deleted malware and reconstruct attacker actions.

---

## Question

**SHA1 Hash?**

### Answer

```text
5b15c9d9ef36cae9f24ce63eebd190ac381bb734
```

### 🧠 Explanation

A SHA1 hash uniquely identifies a file. Analysts search this hash in VirusTotal, MalwareBazaar, or internal threat intelligence databases to determine whether the file is known malware.

> **IOC:** SHA1 Hash

---

# **Task 8 – Windows Event Log Analysis**

## Question

**When was Windows Defender disabled?**

### Answer

**10:25:14 AM**

### 🧠 Explanation

Disabling Windows Defender is a common attacker technique used to evade detection before deploying malware.

---

## Question

**What was the attacker's IP address?**

### Answer

**10.11.90.211**

### 🧠 Explanation

This IP established the successful RDP session before the malware was executed. Analysts correlate RDP logs, authentication logs, firewall events, and endpoint telemetry to verify unauthorized access.

> **IOC:** `10.11.90.211`

---

# **Task 9 – Chronological Order of Attack**

### Attack Timeline

| Step | Activity                                    |
| ---- | ------------------------------------------- |
| 1    | Attacker connected through **RDP**          |
| 2    | Windows Defender was disabled               |
| 3    | **Cursed.rar** was copied to the system     |
| 4    | RAR archive was opened                      |
| 5    | **Cipher.exe** was executed                 |
| 6    | Scheduled Task **CnC** was created          |
| 7    | SSH connections were attempted every minute |
| 8    | Malware and archive were deleted            |

### 🧠 AI SOC Explanation

Building a timeline helps SOC analysts understand **how the attack progressed** from initial access to persistence. Instead of viewing isolated events, analysts correlate timestamps from Event Logs, Prefetch, Amcache, LNK files, and Scheduled Tasks to reconstruct the complete attack chain and identify the attacker's actions.

---

# 🛡 IOC (Indicators of Compromise)

* User: **Aashir**
* Malicious IP: **101.55.125.10**
* Attacker IP: **10.11.90.211**
* Scheduled Task: **CnC**
* Archive: **Cursed.rar**
* Malware: **Cipher.exe**
* SHA1 Hash: **5b15c9d9ef36cae9f24ce63eebd190ac381bb734**
* Windows Defender Disabled (Event ID 5001)
* Successful RDP Login (Event ID 1149)

---

# 🎯 MITRE ATT&CK Mapping

| Technique                   | ATT&CK ID |
| --------------------------- | --------- |
| Remote Services (RDP)       | T1021.001 |
| Scheduled Task Persistence  | T1053.005 |
| Command and Scripting       | T1059     |
| Ingress Tool Transfer       | T1105     |
| Disable Security Tools      | T1562.001 |
| Remote Access Software / C2 | T1071     |

---

# 🔍 SOC Detection Opportunities

* Monitor successful RDP logins (Event ID **1149**).
* Alert when Windows Defender is disabled (Event ID **5001**).
* Detect creation or modification of Scheduled Tasks.
* Monitor execution of unknown executables via Prefetch or EDR.
* Investigate outbound SSH traffic from Windows endpoints.
* Correlate file creation, execution, and persistence events into a single incident.

> **AI SOC Professor Tip:** A single artifact rarely tells the whole story. Skilled SOC analysts **correlate multiple forensic artifacts**—such as Event Logs, Prefetch, Amcache, LNK files, and Scheduled Tasks—to reconstruct the full attack timeline. This evidence-based approach is essential for accurate incident response and root-cause analysis.



### Real-world relevance

In a real SOC, malware is often deleted before analysts receive the affected machine.

Digital forensic artifacts help investigators answer questions such as:

* How did the attacker gain access?
* What files were executed?
* What persistence mechanism was created?
* Which IP addresses communicated with the system?
* What happened before containment?

---

## 🔬 Investigation Summary

The investigation revealed the following attack sequence:

* Attacker established an RDP session.
* Windows Defender was disabled.
* A malicious RAR archive was copied onto the system.
* The archive was extracted.
* A malicious executable (**Cipher.exe**) was launched.
* A scheduled task (**CnC**) was created for persistence.
* The malware attempted SSH communication every minute with a command-and-control server.
* Finally, the attacker removed the malicious files to hide evidence.

Even though the files were deleted, Windows forensic artifacts preserved enough evidence to reconstruct the attack.

---

## 🛡️ Indicators of Compromise (IOCs)

| IOC Type     | Value                                    |
| ------------ | ---------------------------------------- |
| Victim User  | Aashir                                   |
| Malicious IP | 101.55.125.10                            |
| Attacker IP  | 10.11.90.211                             |
| Malware      | Cipher.exe                               |
| Archive      | Cursed.rar                               |
| Persistence  | Scheduled Task (CnC)                     |
| SHA1 Hash    | 5b15c9d9ef36cae9f24ce63eebd190ac381bb734 |

---

## 🛡️ SOC Analyst L1:  Tricky Connection

A beginner focuses on finding malware.

A SOC Analyst focuses on collecting evidence.

This room changed my mindset from:

> "Find the malicious file."

to

> "Reconstruct the attack using available evidence."

This is one of the most important skills in incident response because attackers often attempt to erase their traces.

**Next learning topics:**

* Windows Event IDs
* Sysmon
* Registry Forensics
* Timeline Analysis
* Memory Forensics
* Volatility
* MITRE ATT&CK
* Sigma Detection Rules

---

## 💼 Scenario-Based  Question

**Question**

A Windows endpoint shows no malware during antivirus scanning, but users report suspicious behavior. What would you investigate first as a SOC Analyst L1?

**Answer Concept**

I would not assume the system is clean simply because no malware is present. I would examine Windows Event Logs, Scheduled Tasks, Prefetch, Amcache, Startup locations, network connections, and recent file activity to determine whether malicious activity occurred and whether persistence or attacker artifacts remain. My goal would be to build a timeline of events using forensic evidence before concluding the investigation.

---

## 🎯 Final Reflection

This room taught me that incident response is not about guessing—it is about following evidence. Windows keeps valuable forensic artifacts that help investigators understand attacker behavior even after malware has been deleted. This experience strengthened my understanding of digital forensics and reinforced the importance of evidence-based investigation in a SOC environment.

---

