
# Walkthrough: [Windows](https://tryhackme.com/room/windowsfundamentals1xbx) [Fundamentals 
](https://tryhackme.com/room/windowsfundamentals2x0x)
<img width="96" height="96" alt="image" src="https://github.com/user-attachments/assets/6e97ccd4-66e3-4b3c-a4bd-59fb4417616c" />


> **Room Objective:**
> Build a strong understanding of the Windows operating system by exploring its editions, graphical interface, file system, user accounts, permissions, and essential administrative tools. As an aspiring SOC Analyst, learning Windows is important because most enterprise environments use Windows systems, and many security incidents originate from or target them.

---

## Who is this for?

This room is designed for:

* Beginners learning Windows
* Cybersecurity students
* SOC Analysts
* System Administrators
* Blue Team professionals
* Incident Responders

Since Windows is the most widely used desktop operating system in organizations, understanding how it works is an essential skill for anyone working in cybersecurity.

---

## What is Windows Fundamentals?

Windows Fundamentals introduces the core components of the Microsoft Windows operating system.

Instead of focusing on hacking, this room teaches how Windows is organized, how users interact with it, how files are stored, how permissions work, and where administrators manage the operating system.

It builds the foundation needed before learning Windows logging, Active Directory, PowerShell, Event Viewer, Sysinternals, Windows Defender, and digital forensics.

---

## Where does this apply?

These concepts are used daily in:

* Security Operations Centers (SOC)
* Corporate IT Departments
* Incident Response Teams
* Digital Forensics
* Malware Analysis
* Windows Server Administration
* Enterprise Helpdesk

Almost every Windows security investigation begins with understanding the operating system itself.

---

## When should you learn this?

Windows Fundamentals should be learned before studying:

* Windows Event Logs
* Active Directory
* Sysmon
* Microsoft Defender
* Windows Security
* Registry Analysis
* PowerShell
* Digital Forensics
* Malware Analysis

Without understanding Windows basics, advanced security topics become much harder to understand.

---

## Why is it important?

Many cyber attacks target Windows because it is the most commonly deployed operating system in businesses.

A SOC Analyst must know:

* How Windows normally behaves
* Where important system files are stored
* How user accounts work
* How permissions are managed
* Which administrative tools are commonly used

Knowing what is "normal" makes it easier to identify suspicious activity.

---

## How does it work?

The room gradually introduces Windows by exploring:

* Different Windows editions
* Desktop interface
* Windows file system
* System folders
* User accounts
* User Account Control (UAC)
* Control Panel
* Task Manager

Each topic builds toward understanding how Windows functions in a real enterprise environment.

---


## What confused me?

Before starting this room, I only used Windows as a regular user.

I knew how to install software, browse files, and use applications, but I had never asked questions like:

* Why does Windows have different editions?
* What is NTFS?
* Why is the **System32** folder so important?
* What exactly does User Account Control (UAC) protect?
* Why are there multiple administrator accounts?
* How does Windows manage user permissions?

I also realized that many Windows features I used every day had a security purpose that I never understood before.

---

## What I learned?

This room taught me that Windows is much more than a graphical desktop.

I learned about:

### Windows Editions

Different editions of Windows provide different security features and management capabilities.

Business environments commonly use Windows Professional or Enterprise because they include additional security features that are unavailable in Home editions.

---

### Windows Desktop (GUI)

The Windows desktop is not just a user interface.

It provides access to:

* Start Menu
* Taskbar
* Notification Area
* Search
* Task View

Understanding these components helps identify abnormal behavior during investigations.

---

### Windows File System

The room introduced NTFS (New Technology File System).

NTFS provides important security features including:

* File permissions
* Access control
* Encryption support
* Journaling
* Better reliability

These features help protect data and maintain system integrity.

---

### Windows System Folder

The **Windows** directory contains essential operating system files.

One of the most important folders is:

**System32**

This folder stores:

* System DLLs
* Drivers
* Windows utilities
* Administrative tools

Many malware families target or abuse this folder because it contains trusted system files.

---

### User Accounts and Permissions

Windows uses accounts and groups to control access.

Different users receive different permissions depending on their responsibilities.

This follows the Principle of Least Privilege, reducing the risk of unauthorized changes.

---

### User Account Control (UAC)

UAC is an important Windows security feature.

Instead of allowing every application to run with administrator privileges, Windows asks for confirmation before performing administrative actions.

This helps reduce accidental or malicious system changes.

---

### Control Panel

The Control Panel allows administrators to configure Windows settings.

Although many settings have moved to the newer Settings application, Control Panel remains an important administrative interface.

---

### Task Manager

Task Manager provides real-time information about:

* Running processes
* CPU usage
* Memory usage
* Disk activity
* Startup applications
* Logged-on users

For SOC analysts, Task Manager is often one of the first tools used during endpoint investigations.

---
Task 1 – Windows Editions
Question

What encryption can you enable on Pro that you can't enable in Home?

Answer

BitLocker

🧠  Explanation

BitLocker is Microsoft's full-disk encryption feature available in Windows Pro, Enterprise, and Education editions. It protects data by encrypting the entire drive, preventing unauthorized access if a laptop or hard drive is stolen.

SOC Note: Analysts verify BitLocker status during incident response to assess whether sensitive data is protected.

Task 2 – Desktop (GUI)
Question

Which option hides the Search box?

Answer

Hidden

🧠 Explanation

The Search Box helps users quickly find applications, files, and settings. Hiding it only changes the desktop appearance; search functionality remains available.

Question

Which option hides Task View?

Answer

Show Task View button

🧠 Explanation

Task View displays open applications and virtual desktops. Analysts may use it to observe multiple investigation windows efficiently.

Question

Which icon appears besides Clock and Network?

Answer

Action Center

🧠 Explanation

Action Center displays Windows notifications, security alerts, firewall warnings, and Defender messages that may indicate suspicious activity.

Task 3 – Windows Introduction
Question

Start the Windows Lab Machine.

Answer

Completed
<img width="604" height="474" alt="image" src="https://github.com/user-attachments/assets/bfffe328-a867-4f06-a371-19a897d55ed4" />

🧠 Explanation

The lab provides a safe Windows environment for practicing system administration and security investigations without affecting a production system.

Task 4 – File System
Question

What does NTFS stand for?

Answer

New Technology File System
<img width="723" height="507" alt="image" src="https://github.com/user-attachments/assets/853903cc-df35-4224-8cf3-55eee85f531f" />

🧠 Explanation

NTFS is the default Windows file system. It supports permissions, encryption, compression, auditing, and journaling, making it suitable for enterprise environments.

SOC Note: NTFS permissions are frequently examined during privilege abuse investigations.

Task 5 – Windows\System32  <img width="782" height="589" alt="image" src="https://github.com/user-attachments/assets/499a9d4d-885a-4343-8cc2-b41fd02ea918" />

Question

What is the system variable for the Windows folder?

Answer

%windir%

🧠 Explanation

%windir% points to the Windows installation directory (usually C:\Windows). Environment variables simplify scripts and system administration.

Task 6 – User Accounts
Question

What is the other user account?

Answer

tryhackmebilly

🧠 Explanation

Reviewing user accounts helps identify unauthorized accounts or attackers who created persistence.

Question

Which groups is the user a member of?

Answer

Remote Desktop Users, Users

🧠 Explanation

Group membership determines user permissions. Remote Desktop Users can log in remotely, which analysts monitor for unauthorized access.

Question

Which built-in account allows guest access?

Answer

Guest

🧠 Explanation

The Guest account provides limited access and is usually disabled because attackers may abuse it.

Question

Account Description?

Answer

window$Fun1!
<img width="507" height="190" alt="image" src="https://github.com/user-attachments/assets/df1ebce4-2da4-4268-8a22-27b3286ae318" />

🧠 Explanation

Administrators often use account descriptions for documentation. Analysts review descriptions for suspicious notes or hidden information.

Task 7 – User Account Control (UAC)
Question

What does UAC stand for?

Answer

User Account Control
<img width="392" height="413" alt="image" src="https://github.com/user-attachments/assets/24f623d7-f02c-40ef-b1f1-8ae19b19120b" />

🧠 Explanation

UAC prevents unauthorized administrative changes by prompting users before privileged actions are performed.

SOC Note: Malware often attempts to bypass UAC for privilege escalation.

Task 8 – Settings & Control Panel
Question

Last Control Panel item?

Answer

Windows Defender Firewall
<img width="725" height="323" alt="image" src="https://github.com/user-attachments/assets/b3c9c10c-ee9d-4d5d-a5f6-9cfc4d561a73" />

🧠 Explanation

Windows Defender Firewall filters inbound and outbound traffic, helping block unauthorized network communication.

Task 9 – Task Manager
Question

Shortcut to open Task Manager?

Answer

Ctrl + Shift + Esc
<img width="661" height="578" alt="image" src="https://github.com/user-attachments/assets/8da2164e-07af-4024-b1e6-3b1b69ec209a" />

🧠 Explanation

Task Manager allows analysts to inspect running processes, CPU usage, memory consumption, startup applications, and suspicious executables.

Task 10 – Conclusion
🧠 AI SOC Professor Summary

Windows Fundamentals 1 introduced the Windows interface, file system, user accounts, permissions, Task Manager, UAC, and Control Panel. These components are frequently examined during endpoint investigations.

------------
## How I improved?

Before completing this room, I saw Windows mainly as an operating system used to run applications.

Now I understand that every component has a security purpose.

Instead of simply opening Task Manager, I now think:

* Which process is consuming unusual CPU?
* Why is this application running?
* Should this service normally exist?

Instead of only opening File Explorer, I think:

* Is this file stored in a normal location?
* Does this folder usually contain executable files?
* Are the permissions appropriate?

This room helped me start thinking like an analyst rather than just a Windows user.

---

## Real-World Relevance

Windows systems are involved in many enterprise security incidents.

SOC analysts frequently investigate:

* Unauthorized user accounts
* Privilege misuse
* Suspicious processes
* Malware stored inside Windows folders
* Abnormal services
* Unexpected startup applications
* Unauthorized software installation

Understanding Windows Fundamentals makes these investigations much easier.

Without knowing how Windows normally operates, identifying suspicious activity becomes difficult.

---

# 🛡️ SOC Analyst L1: Mindset 

This room reinforced an important lesson:

> A SOC Analyst does not need to memorize every Windows feature—they need to understand what "normal" looks like.

For example:

* A normal Windows system has expected processes, folders, services, and user accounts.
* An attacker often introduces something unusual, such as a new process, unexpected account, modified permissions, or suspicious executable.

The better I understand normal Windows behavior, the faster I can recognize abnormal activity.

### Tricky Connection

Windows Fundamentals is not just about learning the operating system—it prepares me for future topics such as:

* Windows Event Viewer
* Windows Registry
* Sysinternals Suite
* PowerShell
* Windows Defender
* Active Directory
* Microsoft Defender XDR
* Windows Forensics
* Malware Analysis

Every one of these advanced topics depends on understanding the basics covered in this room.

---

# Critical Thinking

### Scenario

A user reports that their Windows computer has become unusually slow. They also mention seeing several unknown programs running after startup.

As a SOC Analyst L1, what would be your first investigation steps?

### Answer Concept

I would begin by gathering basic information without making assumptions.

First, I would review the running processes to identify any unfamiliar or resource-intensive applications. Next, I would examine startup programs to determine whether unauthorized software is launching automatically. I would also verify the logged-in user, recent system changes, and any security alerts generated by endpoint protection tools.

If suspicious activity is identified, I would document my findings, collect supporting evidence, and escalate the incident according to the organization's response procedures.

The goal is not to immediately conclude that malware is present, but to investigate methodically, distinguish normal from abnormal behavior, and support the incident response process with accurate evidence.

---



Excellent choice. These two rooms are **core Windows knowledge** for every SOC Analyst. Below is the same format you've been using (**Question → Answer → AI SOC Professor Explanation**) in concise, portfolio-friendly notes.

---

# X10THINK + /HUMAN

# **Day-20A | Windows Fundamentals 1 (AI SOC Professor Notes)**

---

# **Task 1 – Windows Editions**

### Question

**What encryption can you enable on Pro that you can't enable in Home?**

### Answer

**BitLocker**

### 🧠 AI SOC Professor Explanation

**BitLocker** is Microsoft's full-disk encryption feature available in Windows Pro, Enterprise, and Education editions. It protects data by encrypting the entire drive, preventing unauthorized access if a laptop or hard drive is stolen.

> **SOC Note:** Analysts verify BitLocker status during incident response to assess whether sensitive data is protected.

---

# **Task 2 – Desktop (GUI)**

### Question

**Which option hides the Search box?**

### Answer

**Hidden**

### 🧠 Explanation

The Search Box helps users quickly find applications, files, and settings. Hiding it only changes the desktop appearance; search functionality remains available.

---

### Question

**Which option hides Task View?**

### Answer

**Show Task View button**

### 🧠 Explanation

Task View displays open applications and virtual desktops. Analysts may use it to observe multiple investigation windows efficiently.

---

### Question

**Which icon appears besides Clock and Network?**

### Answer

**Action Center**

### 🧠 Explanation

Action Center displays Windows notifications, security alerts, firewall warnings, and Defender messages that may indicate suspicious activity.

---

# **Task 3 – Windows Introduction**

### Question

**Start the Windows Lab Machine.**

### Answer

**Completed**

### 🧠 Explanation

The lab provides a safe Windows environment for practicing system administration and security investigations without affecting a production system.

---

# **Task 4 – File System**

### Question

**What does NTFS stand for?**

### Answer

**New Technology File System**

### 🧠 Explanation

NTFS is the default Windows file system. It supports permissions, encryption, compression, auditing, and journaling, making it suitable for enterprise environments.

> **SOC Note:** NTFS permissions are frequently examined during privilege abuse investigations.

---

# **Task 5 – Windows\System32**

### Question

**What is the system variable for the Windows folder?**

### Answer

**%windir%**

### 🧠 Explanation

`%windir%` points to the Windows installation directory (usually `C:\Windows`). Environment variables simplify scripts and system administration.

---

# **Task 6 – User Accounts**

### Question

**What is the other user account?**

### Answer

**tryhackmebilly**

### 🧠 Explanation

Reviewing user accounts helps identify unauthorized accounts or attackers who created persistence.

---

### Question

**Which groups is the user a member of?**

### Answer

**Remote Desktop Users, Users**

### 🧠 Explanation

Group membership determines user permissions. Remote Desktop Users can log in remotely, which analysts monitor for unauthorized access.

---

### Question

**Which built-in account allows guest access?**

### Answer

**Guest**

### 🧠 Explanation

The Guest account provides limited access and is usually disabled because attackers may abuse it.

---

### Question

**Account Description?**

### Answer

**window$Fun1!**

### 🧠 Explanation

Administrators often use account descriptions for documentation. Analysts review descriptions for suspicious notes or hidden information.

---

# **Task 7 – User Account Control (UAC)**

### Question

**What does UAC stand for?**

### Answer

**User Account Control**

### 🧠 Explanation

UAC prevents unauthorized administrative changes by prompting users before privileged actions are performed.

> **SOC Note:** Malware often attempts to bypass UAC for privilege escalation.

---

# **Task 8 – Settings & Control Panel**

### Question

**Last Control Panel item?**

### Answer

**Windows Defender Firewall**

### 🧠 Explanation

Windows Defender Firewall filters inbound and outbound traffic, helping block unauthorized network communication.

---

# **Task 9 – Task Manager**

### Question

**Shortcut to open Task Manager?**

### Answer

**Ctrl + Shift + Esc**

### 🧠 Explanation

Task Manager allows analysts to inspect running processes, CPU usage, memory consumption, startup applications, and suspicious executables.

---

# **Task 10 – Conclusion**

###  Summary

Windows Fundamentals 1 introduced the Windows interface, file system, user accounts, permissions, Task Manager, UAC, and Control Panel. These components are frequently examined during endpoint investigations.

---

> **Room Objective:**
> Build a deeper understanding of Windows administration by exploring built-in management tools, system configuration, computer management, system information, resource monitoring, command-line utilities, and the Windows Registry. This room extends the knowledge from Windows Fundamentals 1 and introduces the tools that IT administrators and SOC Analysts frequently use during troubleshooting and security investigations.

---

## Who is this for?

This room is designed for:

* Cybersecurity beginners
* SOC Analysts
* Blue Team professionals
* Windows Administrators
* Incident Responders
* Digital Forensics learners

Anyone working with Windows systems should understand these administrative tools because they are used daily in enterprise environments.

---

## What is Windows Fundamentals 2?

Windows Fundamentals 2 focuses on the built-in Windows management utilities that help administrators configure, monitor, and troubleshoot systems.

Unlike the first room, which introduced the Windows interface and basic components, this room explains where important system information is stored, how administrators manage services and users, and how Windows provides detailed information about hardware, software, and system performance.

---

## Where does this apply?

These concepts are used in:

* Enterprise SOCs
* IT Support Teams
* Windows Server Administration
* Incident Response
* Digital Forensics
* Endpoint Security
* Malware Investigation

Most Windows security investigations involve one or more of these administrative tools.

---

## When should you learn this?

This room should be completed after learning Windows Fundamentals 1 and before moving to topics such as:

* Windows Event Viewer
* Sysinternals Suite
* PowerShell
* Windows Registry Analysis
* Active Directory
* Windows Defender
* Microsoft Defender XDR
* Windows Forensics

Understanding these tools makes advanced Windows security topics much easier.

---

## Why is it important?

A SOC Analyst investigates Windows endpoints every day.

To understand what happened on a system, an analyst must know:

* How Windows stores system information
* Where services are configured
* How scheduled tasks work
* How resource usage is monitored
* How Windows Registry stores configuration settings
* Which built-in utilities administrators commonly use

Without this knowledge, analyzing alerts and suspicious behavior becomes much more difficult.

---

## How does it work?

The room introduces several important Windows administrative utilities, including:

* System Configuration (MSConfig)
* Advanced System Settings
* User Account Control (UAC)
* Computer Management
* System Information
* Resource Monitor
* Command Prompt
* Registry Editor

Each tool provides different information about the operating system and helps administrators troubleshoot or investigate Windows systems.

---


## What confused me?

Initially, I thought these Windows tools were only meant for system administrators.

I wondered:

* Why are there so many management utilities?
* What is the difference between MSConfig, Computer Management, and Control Panel?
* Why does Windows have both graphical tools and command-line tools?
* Why is the Windows Registry considered so important?
* How does Resource Monitor differ from Task Manager?

After completing the room, I realized that each utility serves a different purpose and provides unique information useful during investigations.

---

## What I learned?

This room helped me understand several important Windows administrative tools.

### System Configuration (MSConfig)

MSConfig is used to manage system startup, boot options, and troubleshooting settings.

It helps administrators diagnose startup problems and identify unnecessary services or applications that load during boot.

---

### User Account Control (UAC)

I learned more about how UAC protects Windows by preventing unauthorized administrative actions.

Instead of allowing every program to make system changes automatically, Windows asks for user confirmation before granting elevated privileges.

This reduces the risk of malware silently modifying the operating system.

---

### Computer Management

Computer Management combines multiple administrative tools into a single console.

It allows administrators to manage:

* Local users and groups
* Shared folders
* Event Viewer
* Device Manager
* Disk Management
* Services
* Scheduled Tasks

This makes it one of the most frequently used tools in enterprise environments.

---

### System Information

System Information provides detailed information about:

* Hardware
* Operating system
* BIOS
* Installed components
* Environment variables
* Drivers

This information is valuable during troubleshooting and incident investigations.

---

### Resource Monitor

Resource Monitor offers deeper visibility into system activity than Task Manager.

It allows administrators to monitor:

* CPU usage
* Memory consumption
* Disk activity
* Network connections

For SOC analysts, unusual resource usage can indicate malware, cryptocurrency miners, or unauthorized applications.

---

### Command Prompt

The Command Prompt enables administrators to execute Windows commands directly.

Utilities like **ipconfig**, **ping**, **netstat**, and **tracert** help investigate networking issues and system configuration.

Learning these commands is essential because many security investigations rely on command-line tools.

---

### Windows Registry

The Windows Registry is a centralized database that stores operating system and application configuration settings.

Many attackers modify Registry keys to:

* Maintain persistence
* Execute malware during startup
* Disable security controls

Understanding the Registry is therefore an important skill for malware analysis and digital forensics.

---


# **Task 1 – Introduction**

### Question

**Start the Lab Machine**

### Answer

**Completed**
<img width="604" height="474" alt="image" src="https://github.com/user-attachments/assets/5c139e65-6e84-41bc-a50e-7d78f39a7111" />

### 🧠 Explanation

This lab focuses on Windows administrative tools used by system administrators and SOC analysts.

---

# **Task 2 – System Configuration**

### Question

**Service listing Sysinternals as manufacturer?**

### Answer

**PsShutdown**

### 🧠 Explanation

PsShutdown is part of Microsoft's Sysinternals Suite used for remote shutdown and administrative management.

---

### Question

**Windows license registered to?**

### Answer

**Windows User**

### 🧠 Explanation

Registration information identifies the Windows installation owner.

---

### Question

**Windows Troubleshooting command?**

### Answer

```text
C:\Windows\System32\control.exe /name Microsoft.Troubleshooting
```

### 🧠 Explanation

This command opens the Windows Troubleshooting utility for diagnosing common system issues.

---

### Question

**Control Panel executable?**

### Answer

**control.exe**

### 🧠 Explanation

`control.exe` launches the Windows Control Panel directly from Run or Command Prompt.

---

# **Task 3 – UAC Settings**

### Question

**Command to open UAC Settings?**

### Answer

**UserAccountControlSettings.exe**
<img width="704" height="538" alt="image" src="https://github.com/user-attachments/assets/7948de3d-f74c-4187-a4a0-198061f91d92" />

### 🧠 Explanation

This utility changes the User Account Control notification level, affecting how Windows prompts for administrative privileges.

---

# **Task 4 – Computer Management**

### Question

**Computer Management command?**

### Answer

**compmgmt.msc**

### 🧠 Explanation

Computer Management provides access to Event Viewer, Device Manager, Disk Management, Services, Local Users, and Shared Folders.

---

### Question

**When does npcapwatchdog run?**

### Answer

**At system startup**

### 🧠 Explanation

Scheduled tasks automatically execute programs. Analysts investigate unexpected startup tasks for malware persistence.

---

### Question

**Hidden shared folder?**

### Answer

**sh4r3dF0Ld3r**

### 🧠 Explanation

Hidden network shares are common in enterprise environments but can also be abused by attackers.

---

# **Task 5 – System Information**
<img width="1085" height="702" alt="image" src="https://github.com/user-attachments/assets/24eaec35-bd07-4096-a5db-096e6d7f2da8" />

### Question

**System Information command?**

### Answer

**msinfo32.exe**

### 🧠 Explanation

System Information displays hardware, BIOS, drivers, installed software, environment variables, and operating system details.

---

### Question

**System Name?**

### Answer

**THM-WINFUN2**

### 🧠 Explanation

The computer name uniquely identifies the system during investigations.

---

### Question

**ComSpec value?**

### Answer

```text
%SystemRoot%\system32\cmd.exe
```

### 🧠 Explanation

ComSpec identifies the default Windows Command Prompt executable.

---

# **Task 6 – Resource Monitor**

### Question

**Resource Monitor command?**

### Answer

**resmon.exe**
<img width="1261" height="804" alt="image" src="https://github.com/user-attachments/assets/bcb6f967-d2e8-47b0-af52-559890603941" />

### 🧠 Explanation

Resource Monitor displays CPU, Memory, Disk, and Network usage, helping analysts identify suspicious resource consumption.

---

# **Task 7 – Command Prompt**

### Question

**Full command for Internet Protocol Configuration?**

### Answer

```text
C:\Windows\System32\cmd.exe /k %windir%\system32\ipconfig.exe
```

### 🧠 Explanation

This command launches Command Prompt and automatically executes `ipconfig`.

---

### Question

**How to display detailed network information?**

### Answer

```cmd
ipconfig /all
```

### 🧠 Explanation

`ipconfig /all` displays IP addresses, DNS servers, MAC addresses, DHCP details, and network adapter information.

> **SOC Note:** Useful for identifying suspicious DNS settings or network misconfigurations.

---

# **Task 8 – Registry Editor**

### Question

**Registry Editor command?**

### Answer

**regedt32.exe**
![Uploading image.png…]()

### 🧠 Explanation

Registry Editor manages Windows Registry settings. Malware often modifies registry keys to establish persistence.

---

# **Task 9 – Conclusion**

###  Summary

Windows Fundamentals 2 introduced administrative tools such as MSConfig, Computer Management, System Information, Resource Monitor, Command Prompt, Registry Editor, and scheduled tasks. These utilities are essential for SOC analysts investigating Windows endpoints because they help identify suspicious processes, startup persistence, system configuration changes, and evidence of attacker activity.

---

## How I improved?

This room changed how I look at Windows administration.

Earlier, I mainly relied on the graphical interface for everyday tasks.

Now I understand that Windows provides powerful built-in utilities for monitoring, troubleshooting, and investigating systems.

Instead of thinking:

> "Where is this setting?"

I now think:

* Which Windows tool can provide the information I need?
* Which utility would help verify this alert?
* Could this system behavior indicate malicious activity?

This mindset is much closer to how SOC analysts approach investigations.

---

## Real-World Relevance

These Windows tools are used regularly during security investigations.

For example:

* Resource Monitor helps identify suspicious network connections.
* Computer Management is used to investigate unauthorized user accounts.
* System Information helps verify hardware and operating system details.
* Command Prompt assists in network troubleshooting.
* Registry Editor is often examined during malware investigations.
* MSConfig helps identify unwanted startup programs.

Understanding these tools allows analysts to investigate Windows endpoints more effectively and respond to incidents with greater confidence.

---

# 🛡️ SOC Analyst L1: Mindset 

This room taught me that Windows contains many built-in investigative tools before using advanced security software.

As a SOC Analyst, my responsibility is not only to monitor SIEM alerts but also to understand what is happening on the affected endpoint.

### Tricky Connection

Windows Fundamentals 2 prepares me for the next stage of my learning journey:

* Windows Event Viewer
* Windows Logs
* Sysinternals Suite
* PowerShell
* Active Directory
* Windows Security Logs
* Microsoft Defender
* Endpoint Detection and Response (EDR)
* Digital Forensics

All these topics rely on understanding the administrative tools introduced in this room.

---

# Critical Thinking

### Scenario

A user reports that their Windows system becomes very slow immediately after startup. An EDR alert also indicates that an unknown executable is establishing outbound network connections.

As an L1 SOC Analyst, what would you investigate first?

### Answer Concept

I would begin by reviewing the endpoint using built-in Windows tools.

First, I would check startup programs and running processes to identify any unfamiliar applications. Next, I would use Resource Monitor to examine active network connections and determine whether any suspicious process is communicating externally. I would also review Computer Management for newly created user accounts or unexpected scheduled tasks. If necessary, I would collect supporting evidence, document my observations, and escalate the incident to the L2 SOC or Incident Response team.

The objective is to validate the alert using multiple sources of evidence rather than relying on a single indicator.

------
## 🛡 SOC Analyst Key Detection Focus

* Monitor **Event Viewer** logs for suspicious events.
* Investigate **Task Manager** for unknown processes.
* Review **Scheduled Tasks** for persistence.
* Check the **Registry** for malicious Run keys.
* Verify **User Accounts** and group memberships.
* Use **ipconfig /all** to inspect network settings.
* Confirm **BitLocker** and **Windows Defender Firewall** status.
* Examine **System32** and `%windir%` for unauthorized files or changes.

------

## 🎯 Final Reflection 

Completing Windows Fundamentals 1 and Windows Fundamentals 2 gave me a solid understanding of how the Windows operating system works beyond everyday use. I learned about the Windows interface, file system, user accounts, permissions, administrative tools, system configuration, Resource Monitor, Command Prompt, and the Windows Registry.

As an aspiring SOC Analyst L1, I now understand that investigating security incidents starts with knowing how Windows normally behaves. These two rooms have built a strong foundation for my next learning topics, including Windows Event Logs, Sysinternals, Active Directory, PowerShell, Microsoft Defender, and Windows Forensics, which are essential skills for detecting and responding to threats in enterprise environments.

