
#  Walkthrough: [Registry Persistence Detection](https://tryhackme.com/room/registrypersistencedetection?vccr=1)
<img width="256" height="256" alt="image" src="https://github.com/user-attachments/assets/503a4038-4145-40e0-b264-e524db17e06b" />

As part of my SOC Analyst L1 learning journey, I completed the **Registry Persistence Detection** room to understand how attackers maintain access to a compromised Windows system even after a reboot.

My biggest confusion before starting this room was:

> **How can malware continue running after the computer restarts without the user launching it manually?**

I also wanted to understand how Windows Registry persistence works, why attackers modify Registry keys, and how SOC analysts detect these changes during investigations.

---

### **Who is this for?**

* SOC Analysts
* Incident Responders
* Threat Hunters
* Malware Analysts
* Windows Security Administrators

### **What is Registry Persistence?**

Registry persistence is a technique attackers use to automatically execute malicious files whenever Windows starts or a user logs in. Instead of running malware manually, attackers modify Registry entries so Windows launches the malicious program automatically.

### **Where does this apply?**

* Windows endpoints
* Enterprise laptops
* Corporate desktops
* Active Directory environments
* Digital forensic investigations

### **When should I learn this?**

After learning Windows fundamentals and before studying malware analysis, DFIR, Sysmon, or advanced threat detection.

### **Why is it important?**

Many attackers do not stop after gaining initial access. They create persistence so they can return later without exploiting the system again. Detecting persistence is one of the first tasks during incident response.

### **How does it work?**

Windows automatically checks several Registry locations during startup and user logon.

Attackers abuse these locations by inserting malicious executables or batch files.

------


# **Task 2 – Introduction to Malware Persistence Mechanisms**

### **Question**

**What is the value "Name" of the suspicious registry entry that runs during startup?**

### **Answer**

**(Default)**
# HKEY_LOCAL_MACHINE > Software > Microsoft > Windows > CurrentVersion > Run
<img width="749" height="186" alt="image" src="https://github.com/user-attachments/assets/ac4c9444-86c2-4b03-961c-5c263bd9afc5" />

### 🧠  Explanation

`(Default)` is the default value name inside a Windows Registry key. Attackers sometimes modify this value to execute malware automatically when Windows starts. SOC analysts should always verify if the default value points to a trusted application.

---

### **Question**

**What is the value "Data" of the suspicious registry entry?**

### **Answer**

```text
C:\Users\Administrator\AppData\Local\bd84\24d9.bat
```

### 🧠 Explanation

The **Data** field contains the command or file that Windows executes during startup. In this case, it points to a **batch (.bat) file** inside the user's AppData folder, which is unusual and often used by malware to persist after reboot.

> **SOC Note:** Startup scripts located in **AppData**, **Temp**, or hidden folders are highly suspicious.

---

### **Question**

**What string is displayed when the suspicious file runs?**

### **Answer**

```text
pleaseletmepersist
```

### 🧠 Explanation

The displayed string confirms that the batch file executed successfully. During real investigations, analysts look for unexpected console messages, scripts, or processes that indicate malware activity.

---

# **Task 3 – Introduction to the AutoRuns PowerShell Module**

### **Question**

**Which function displays Autorun entries?**

### **Answer**

```powershell
Get-PSAutorun
```

### 🧠 Explanation

This command lists all startup locations monitored by AutoRuns, helping analysts identify programs that launch automatically when Windows boots or users log in.

---

### **Question**

**Which function creates a baseline?**

### **Answer**

```powershell
New-AutoRunsBaseLine
```

### 🧠 Explanation

A **baseline** records the current trusted autorun entries. Later, analysts compare new scans against this baseline to detect unauthorized changes or malware persistence.

---

### **Question**

**Which function compares two baselines?**

### **Answer**

```powershell
Compare-AutoRunsBaseLine
```

### 🧠 Explanation

This command compares an old baseline with a new one and highlights added, removed, or modified autorun entries, making it easier to detect persistence after an attack.

---

# **Task 4 – Filtering AutoRuns Entries**

### **Question**

**Which parameter filters Boot Execute entries?**

### **Answer**

```powershell
-BootExecute
```

### 🧠 Explanation

This filter displays startup programs that execute during the early Windows boot process. Malware may abuse these entries to load before security software.

---

### **Question**

**How many Boot Execute entries were found?**

### **Answer**

**1**

### 🧠 Explanation

Only one entry was present. Analysts should verify whether it belongs to Windows or an unknown application.

---

### **Question**

**Which parameter filters Print Monitor DLLs?**

### **Answer**

```powershell
-PrintMonitorDLLs
```

### 🧠 Explanation

Print Monitor DLLs manage printer communication. Malware occasionally hijacks these DLLs to achieve persistence.

---

### **Question**

**How many Print Monitor entries were found?**

### **Answer**

**5**

### 🧠 Explanation

Multiple printer monitor entries are common, but analysts should investigate unknown or unsigned DLLs.

---

### **Question**

**Which parameter verifies digital signatures?**

### **Answer**

```powershell
-VerifyDigitalSignature
```

### 🧠 Explanation

Digital signatures confirm whether a file is signed by a trusted vendor. Unsigned startup files deserve additional investigation.

---

### **Question**

**How many entries were unsigned?**

### **Answer**

**3**

### 🧠 Explanation

Unsigned files are not always malicious, but they are common indicators of suspicious or unauthorized software.

---

### **Question**

**Can PowerShell answer this without Out-GridView?**

### **Answer**

**Yes**

### 🧠 Explanation

PowerShell filtering and scripting allow analysts to count unsigned entries directly from the command line, making automation possible in enterprise environments.

---

# **Task 5 – Comparing to a Baseline**

### **Question**

**Which Registry key contained another suspicious entry?**

### **Answer**

```text
HKLM:\SOFTWARE\Microsoft\Windows NT\CurrentVersion\Winlogon
```

### 🧠 Explanation

The **Winlogon** Registry key controls the Windows logon process. Malware frequently modifies this location so malicious code executes every time a user logs in.

---

### **Question**

**What was the suspicious Registry value name?**

### **Answer**

```text
Userinit
```

### 🧠 Explanation

`Userinit` normally launches Windows initialization programs. Attackers append their own executable or script to this value for persistence.

---

### **Question**

**What was the suspicious Registry value data?**

### **Answer**

```text
C:\Windows\system32\userinit.exe,C:\Users\Administrator\AppData\Local\THM\789a.bat
```

### 🧠 Explanation

The legitimate `userinit.exe` is followed by a malicious batch file. Windows executes both, allowing malware to run while appearing legitimate.

---

### **Question**

**Which category did AutoRuns assign?**

### **Answer**

**Logon**

### 🧠 Explanation

The malware executes during user logon, making this a **Logon Persistence** technique commonly used by attackers.

---

### **Question**

**What string appeared when the malicious file executed?**

### **Answer**

```text
letmestaymyfriend
```

### 🧠 Explanation

This confirms that the malicious persistence mechanism successfully executed. In real-world investigations, analysts would observe malware behavior instead of challenge messages.

---

# **Task 6 – Conclusion**

### 🧠 Summary

This room taught how attackers maintain long-term access by abusing **Windows Registry startup locations** and **Winlogon entries**. I learned to use the **PowerShell AutoRuns module** to enumerate autorun locations, create trusted baselines, compare changes, verify digital signatures, and detect suspicious persistence mechanisms. These skills are essential for malware analysis, endpoint investigations, and incident response.

---

# 🛡 IOC Examples

* Batch files in **AppData**
* Modified **Winlogon\Userinit**
* New Registry Run keys
* Unsigned startup executables
* Unexpected `.bat`, `.vbs`, or `.ps1` files
* Startup entries pointing to user profile folders

---

# 🎯 MITRE ATT&CK Mapping

| Technique                          | ATT&CK ID     |
| ---------------------------------- | ------------- |
| Registry Run Keys / Startup Folder | **T1547.001** |
| Boot or Logon Autostart Execution  | **T1547**     |
| PowerShell                         | **T1059.001** |
| Scheduled Task / Job               | **T1053**     |

---

# 🔍 SOC Detection Opportunities

* Monitor Registry modifications in **Run**, **RunOnce**, and **Winlogon** keys.
* Alert on startup entries launching from **AppData** or **Temp** folders.
* Detect unsigned executables configured to start automatically.
* Compare AutoRuns baselines regularly to identify new persistence mechanisms.
* Investigate unexpected changes to `Userinit`, `Shell`, or other critical logon values.

> **AI SOC Professor Tip:** Persistence is one of the first things attackers establish after gaining access. During an investigation, always ask: **"What allows this malware to survive a reboot?"** Checking Registry autoruns, scheduled tasks, services, startup folders, and Winlogon entries should be a standard part of every Windows endpoint investigation.

---


## **What confused me?**

Initially, I believed malware only runs when a user opens an infected file.

After this room, I realized attackers can modify Registry startup locations so malware executes automatically after every reboot.

I was also confused about:

* Why attackers choose the Registry instead of creating scheduled tasks.
* How investigators know whether a Registry entry is legitimate or malicious.
* Why the **Userinit** value is commonly abused.

## **What I learned?**

This room introduced several important concepts:

* Windows Registry persistence mechanisms
* Startup and Logon Registry locations
* AutoRuns PowerShell Module
* Baseline comparison
* Digital signature verification
* Detecting suspicious startup entries

Important PowerShell commands I learned:

* `Get-PSAutorun`
* `New-AutoRunsBaseLine`
* `Compare-AutoRunsBaseLine`

I also learned how filtering Autoruns categories helps analysts quickly identify suspicious persistence locations.

## **How I improved?**

Previously, I focused only on malware detection.

Now I understand that detecting **how malware survives** is equally important.

Instead of asking:

> "Is malware running?"

I now ask:

> "Why is it running every time Windows starts?"

This small change represents a more investigative SOC mindset.

## **Real-world relevance**

In an enterprise environment, attackers frequently establish persistence after initial access.

A SOC Analyst may receive alerts involving:

* Modified Registry startup keys
* Unsigned startup programs
* New Winlogon entries
* Unexpected Autoruns artifacts

Using baseline comparisons and digital signature verification helps distinguish normal software from suspicious persistence mechanisms, allowing analysts to prioritize investigations.

---

# 🛡️ SOC Analyst L1: Mindset & Tricky Connection

## **Tricky Connection**

Attackers think:

> "How can I stay on this machine after the reboot?"

A SOC Analyst thinks:

> "What startup mechanism allowed this process to execute automatically?"

That difference changes your mindset from offensive techniques to defensive investigation.

When I see a suspicious process now, I know the investigation should include:

* Startup Registry keys
* Scheduled Tasks
* Startup folders
* Windows Services
* WMI persistence
* Browser extensions

These are all common persistence mechanisms.

### **Next Learning Step**

To build on this room, I plan to study:

1. Windows Registry Fundamentals
2. Sysinternals Autoruns
3. Sysmon Event IDs
4. Windows Event Logs
5. Scheduled Task Persistence
6. Windows Services
7. MITRE ATT&CK – **T1547 (Boot or Logon Autostart Execution)**

These topics will help me understand how persistence appears in enterprise logs and SIEM platforms.

---

# 💼 Scenario-Based  Question

### **Question**

During routine monitoring, Microsoft Defender reports that the **Userinit** Registry value has been modified to include a batch file located inside a user's AppData folder. The user says they never created the file.

As a SOC Analyst L1, how would you investigate?

### **Answer Concept**

I would first validate the alert by reviewing the modified Registry value and confirming whether the batch file is legitimate. Next, I would verify the file path, check its digital signature and hash, and determine when it was created. I would review Windows Event Logs, Sysmon logs, and Defender alerts for related activity, identify the parent process that made the Registry change, and check whether the same persistence exists on other endpoints. If the modification appears unauthorized, I would isolate the affected system if required, document my findings, and escalate the incident according to the organization's incident response process.

---

## 📝 Final Reflection

This room showed me that malware detection is only part of a SOC Analyst's job. Understanding **persistence** is equally important because attackers often try to survive system reboots and maintain long-term access.

Learning to identify suspicious Registry entries, compare Autoruns baselines, and verify digital signatures has strengthened my Windows forensic skills. This knowledge forms a strong foundation for future topics such as malware analysis, threat hunting, digital forensics, and incident response.
