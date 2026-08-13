# Quasar RAT: Inside the Threat

## Introduction
Quasar RAT is a malware family written in .NET which is used by a variety of attackers. The malware is fully functional and open source, and is often packed to make analysis of the source more difficult.

Quasar RAT is a lightweight, multifunctional Windows Remote Access Trojan whose open-source code allows attackers to heavily customize it.

Origin: 
* Started as the legitimate **xRAT** remote-administration tool in **2014**; renamed **Quasar** in **2015**.
* Why attackers use it: Open-source + highly customizable + broad remote-control capabilities.
 * APT usage: Associated in the provided source with groups including **Gaza Cybergang**, **Patchwork**, and **APT10**.
* Typical delivery: Phishing emails with malicious attachments.
Persistence: **Scheduled Tasks and Registry keys**.
* Capabilities: Command execution, file manipulation/download, process control, screenshots, webcam recording, keylogging, password dumping, and remote desktop functionality.
* APT10 variant: The source describes customized Quasar variants targeting organizations in **Japan**, with additional plugin downloading and modified communication/encryption routines.
* MITRE ATT&CK: The source highlights Execution, Persistence, Privilege Escalation, particularly T1053 – Scheduled Task/Job.
* Detection: Sigma rules can be used to detect relevant Quasar activity.

## NOTE: Entire analysis was carried out on an isolated Win10 virtual machine environment.

# Static Analysis
## Toolkit
* Ghidra
* CAPA
* dnSpy


## Using Ghidra for generic information

<img width="806" height="654" alt="111" src="https://github.com/user-attachments/assets/961012e7-4b39-4088-a251-89c73315f2cc" />


``` 
Program Name: 525b378c9abf58b083365cd178a7a611c19ae0107346a714df6ba450eed172ab.exe
Executable Format: Portable Executable (PE)
PE Property[Assembly Version]: 1.4.1.0
PE Property[FileDescription]: Quasar Client
PE Property[InternalName]: Client. exe
```
## Using CAPA to identify the capabilities of the PE along with MITRE ATT&CK mapping

## String set 1
<img width="1461" height="400" alt="4" src="https://github.com/user-attachments/assets/93a26421-485a-4f14-8a43-cbd0dbd50fcf" />

### Strings identified in the PE
```
"BraveSoftware\Brave-Browser\User Data\Default\Login Data"
"Google\Chrome\User Data\Default\Login Data"
"Microsoft\Edge\User Data\Default\Login Data"
```
### MITRE ATT&CK Tactics used
``Credential Access``
### Technique used
`` T1555: Credentials from Password Stores``
### Sub-technique used
`` T1555.003 Credentials from Web Browsers ``
### Explanation

* The PE file contains strings referencing the Login Data databases used by **Chromium-based browsers** such as **Brave**, **Google Chrome**, and **Microsoft Edge**. 
* These databases can contain stored website usernames and encrypted passwords.
* This is consistent with **T1555.003 (Credentials from Web Browsers)**, where an adversary may access browser credential stores and attempt to decrypt or extract saved credentials. 
* On Windows, Chrome/Edge/Brave use browser databases alongside Windows-protected encryption mechanisms.
* The strings alone do not prove credential theft. They indicate that the malware knows the locations of browser credential stores; additional behavioral or code analysis would be needed to confirm actual credential extraction.

## String set 2

<img width="1920" height="587" alt="7" src="https://github.com/user-attachments/assets/77d16c3d-1ef5-4e45-8bdb-8813f22371fe" />

### Registry Key manipulation can be identified in the strings. 
### We will analyse actual registy key changes in Dynamic analysis.

## String set 3

<img width="1432" height="529" alt="8-" src="https://github.com/user-attachments/assets/7adb786f-611b-423e-ab0b-22dedb689a49" />

### Strings identified in the PE
```
"schtasks"
"/create /tn \""
" /sc ONLOGON /tr "
"/rl HIGHEST /f" 
```
### MITRE ATT&CK Tactics used
``Persistence``
### Technique used
`` T1053: Scheduled Task/Job``
### Sub-technique used
`` T1053.005	Scheduled Task ``
### Explanation
* The strings **schtasks** and **/create /tn** indicate that the PE may interact with Windows Task Scheduler to create a scheduled task. 
* Attackers commonly abuse scheduled tasks to achieve persistence by automatically executing malicious programs at startup or at specified times.
* However, strings alone do not confirm that a scheduled task is actually created; further behavioral or code analysis is required to verify execution.

### Function analysis with dnSpy
<img width="395" height="165" alt="9" src="https://github.com/user-attachments/assets/f6c6948d-f1f7-427b-ae5c-b352858c718a" />

Since the PE is written in .NET, the **Quasar.Common.Messages.ReverseProxy** function hints towards a possible C2 communication.

# Dynamic Analysis
## Toolkit
* Event Viewer [Sysmon]
* Process Manager
* Wireshark
### Analyzing endpoints with wireshark

<img width="1021" height="181" alt="image" src="https://github.com/user-attachments/assets/6e241bfa-5b25-4dbc-9be6-cdcde5607915" />

No signs of C2 communication found, as all IP addresses are internal.

### Behavioral analysis using ProcMon and SysMon

<img width="751" height="1039" alt="image" src="https://github.com/user-attachments/assets/7ee0e8d5-cd7d-41fc-b26b-404f6c6d7794" />

Looks like our running malware executable is interacting with many windows files, especially .dlls

<img width="720" height="1042" alt="image" src="https://github.com/user-attachments/assets/17a59428-5e37-4174-bfa0-48cedc9c8cb3" />

Also we can observe many Registry key queries been carried out. Registry changes are less, but queries are more for System Information Discovery.

<img width="606" height="419" alt="image" src="https://github.com/user-attachments/assets/23d9bb4f-7b47-4801-8abf-e61a484401bf" />

Here we can observe in SysMon,  a registry change being made. Lets visit the registry path and observe the changes.

<img width="955" height="610" alt="image" src="https://github.com/user-attachments/assets/3eacc093-3340-4670-82d5-8c4078e44841" />

Visitng the path indicated by SysMon, we can see actual changes made by the running PE.
Sysmon Event ID 13 showed svchost.exe creating a REG_BINARY entry under the Windows AppCompat/PCA registry store for the analyzed executable. 
This appears to be normal Windows compatibility tracking, not confirmed malicious registry modification.

### Confirming the strings claiming persistence, on Task Scheduler.

<img width="720" height="536" alt="image" src="https://github.com/user-attachments/assets/fd6a32d4-1bfd-4bd2-a4a1-07c5c8349904" />

From the investigation, there is no sign of scheduled task being scheduled for persistence.

# Creating a YARA rule

```
rule Quasar Rat
{
    meta:
        description = "Detects PE files containing browser credential-store and Task Scheduler indicators"
        author = "Dishant"
        date = "2026-08-14"

    strings:
        $chrome   = "Google\\Chrome\\User Data\\Default\\Login Data" ascii wide
        $edge     = "Microsoft\\Edge\\User Data\\Default\\Login Data" ascii wide
        $brave    = "BraveSoftware\\Brave-Browser\\User Data\\Default\\Login Data" ascii wide
        $schedule1 = "/sc ONLOGON /tr" ascii wide
        $schedule2 =  " /rl HIGHEST /f " ascii wide
        $schtasks = "schtasks" ascii wide nocase
        $create   = "/create /tn" ascii wide nocase

    condition:
        any of ($chrome, $edge, $brave, $schtasks, $create, $schedule1, $schedule2,  )
}

```

# Virus Total Results

<img width="1920" height="953" alt="21212121212" src="https://github.com/user-attachments/assets/5feb49d2-0ab1-4457-82ab-e71899ae0800" />
To be honest, I could not find much artifacts to prove the PE this dangerously suspicious.

# CONCLUSION: MY TAKE ON THE INVESTIGATION

#### Overall, the sample showed some suspicious behavior, especially possible browser credential access and Scheduled Task activity. 
#### However, not every finding was malicious for example, the registry activity appeared to be normal Windows behavior.
##### My main takeaway is that strings alone don't prove malware behavior. 
#### They are useful indicators, but they should be confirmed with dynamic analysis and other evidence before drawing conclusions.











