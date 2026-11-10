

# CASE-001 — Lazarus Group

**Date:** 11/10/2026  
**Research focus:** Threat intelligence · Malware analysis · Threat hunting · Detection engineering

---

## Introduction

This blog presents an in-depth investigation of the Lazarus Group — its origins, motivations, operations, tactics, techniques and procedures (TTPs), and malware. It examines the risks the group poses to organisations across industries and sizes: why they become targets, what attackers gain, and the impact of a successful compromise.

The goal is to understand the group's activities, analyse its capabilities, and evaluate the threat landscape it creates.

---

## Background

Lazarus Group is a North Korean state-sponsored cyber threat group, believed to have formed around **2009**. It is attributed to North Korea's **Reconnaissance General Bureau (RGB)** and is associated with cyber espionage, cyberwarfare, and financial theft.

### Group Profile

| Attribute | Details |
|---|---|
| Formation | c. 2009 |
| Associated member | Park Jin Hyok (identified in connection with Lazarus operations) |
| Type | Advanced Persistent Threat (APT) |
| Purpose | Financial theft, cyber espionage, cyberwarfare |
| Location | Potonggang District, Pyongyang, North Korea (alleged) |
| Parent organisations | Lab 110 and Bureau 121 (cyberwarfare units of North Korea's RGB) |
| Affiliations | Bureau 121, Unit 180, Andariel |

### Associated Groups

**1. BlueNoroff (APT38)**
- Financially motivated operations.
- Targets banks, financial institutions, and cryptocurrency businesses.
- Associated with fraudulent SWIFT transfers, including the Bangladesh Bank heist (2016).

**2. Andariel**
- Targets South Korean government, military, and business sectors.
- Also associated with financial cybercrime.

---

## Notable Campaigns & Attacks

### 1. Binance Crypto Heist (October 2026)
- **Target:** Binance cryptocurrency exchange.
- **Attack:** Compromised a DevOPS engineer computer, accessed internal systems, carried out transactions.
- **Impact:** ~$6.36 billion stolen. Largest cryptocurrency exchange breach on record.
- **Attribution:** FBI officially attributed to **TraderTraitor** (Lazarus sub-unit), October 31, 2026.

### 2. Operation Dream Job (2020–ongoing)
- **Target:** Defence, aerospace, and government organisations (US, Israel, Australia, France, Germany, India, Russia, Brazil).
- **Attack:** Fake recruiter job offers via LinkedIn/Telegram → trap PDFs → kernel rootkit + persistent backdoor for long-term espionage.
- **Impact:** Ongoing theft of proprietary defence and drone-manufacturing data. No public financial figure.
- **Attribution:** Check Point, ESET, Mandiant, MITRE ATT&CK (C0022). First exposed by ClearSky in 2020.
- **Notable:** 2026 wave used zero-day CVE-2026-68820 (Windows AFD.sys) + post-quantum C2 encryption.   

### 3. Bybit Exchange Hack (February 2025)
- **Target:** Bybit cryptocurrency exchange.
- **Attack:** Supply-chain compromise of Safe{Wallet} (third-party multisig platform). Malicious JavaScript injected into the Safe UI caused signers to approve a forged transaction that transferred cold-wallet ownership.
- **Impact:** ~$1.5 billion (401,347 ETH + stETH) stolen.
- **Attribution:** FBI confirmed **TraderTraitor / Lazarus Group** (PSA250226).

### 4. Ronin Network / Axie Infinity (March 2022)
- **Target:** Ronin Bridge (Axie Infinity blockchain).
- **Attack:** Spear-phishing via fake job recruiter; compromised validator nodes to forge bridge transactions.
- **Impact:** ~$620 million stolen.
- **Attribution:** FBI and OFAC confirmed **Lazarus Group / APT38** (April 2022 sanctions).

### 5. WannaCry Ransomware (May 2017)
- **Target:** Organisations worldwide.
- **Attack:** Self-propagating ransomware exploiting the EternalBlue SMB vulnerability (stolen NSA exploit).
- **Impact:** ~230,000–300,000 computers across 150 countries. Estimated $4–8 billion in total damage. Severe disruption to UK NHS, TSMC, Renault, Deutsche Bahn.
- **Attribution:** FBI and UK NCSC attributed to **Lazarus Group**.

### 6. Bangladesh Bank Heist (February 2016)
- **Target:** Bangladesh Bank (central bank).
- **Attack:** Compromised SWIFT credentials used to initiate fraudulent transfer requests to the Federal Reserve Bank of New York.
- **Impact:** $81 million successfully transferred (of $951 million attempted). Partially recovered.
- **Attribution:** FBI and US Treasury attributed to **Lazarus Group / APT38**.   
## Why the Lazarus Group Operates

## One Core Reason: Money

North Korea is one of the most sanctioned countries on Earth. It can't trade freely, can't borrow from global banks, and can't access SWIFT. The regime needs **hard currency** to survive, and cybercrime is the cheapest, most deniable way to get it.

## Where the Money Goes

- **~50% of North Korea's foreign currency income** comes from cyber theft (UN Panel of Experts)
- **~40–50% of its nuclear and missile weapons programs** are funded by stolen funds (US Deputy National Security Adviser Anne Neuberger)
- Cumulative theft: **~$12 billion since 2016** (TRM Labs, post-Binance hack)
- **$7.4 billion stolen in 2026 alone** — 85% of all global crypto hack losses that year
- The October 2026 Binance hack alone ($6.36B affected, $3B stolen successfully) nearly doubled their total overnight

## The Regime's Three Strategic Goals (in order of priority)

1. **Regime survival** — keeping the Kim family dynasty in power
2. **Self-sufficiency** — being free from outside economic interference
3. **Korean unification** — under North Korean control (long-term, unlikely near-term)

## Why Cyber Specifically?

| Advantage | Why It Matters |
|-----------|----------------|
| **Cheap** | A few hundred hackers cost a fraction of a conventional military |
| **Deniable** | A cyberattack rarely triggers a war; a missile launch does |
| **Sanction-proof** | Stolen crypto can't be traced through traditional banking |
| **Asymmetric** | Lets a poor country project power against the US, South Korea, Japan |

## Kim Jong Un's Own Words

He called cyber capabilities an **"all-purpose sword"** — a tool that can spy, destroy, and generate revenue simultaneously, with minimal risk of retaliation.

---

> **In one line:** The Lazarus Group exists because a sanctioned, impoverished regime needs money to build nuclear weapons and survive, and hacking is the cheapest, most deniable way to steal it from the global financial system.   
# How the Lazarus Group Operates

## The Core Playbook (5 Stages)

| Stage | What They Do | Example |
|-------|-------------|---------|
| **1. Recon** | OSINT on targets — LinkedIn profiles, GitHub repos, job postings, blockchain wallet structures | Mapping a crypto exchange's entire engineering team |
| **2. Social Engineering** | Fake job offers, recruiter personas, "coding challenges" via LinkedIn/Telegram/Calendly | The Ronin hack ($625M) started with a fake LinkedIn recruiter |
| **3. Initial Access** | Victim runs a "coding test" (Python script) or pastes a "fix" command into their terminal → backdoor installed | Bybit hack: Safe{Wallet} dev workstation compromised |
| **4. Lateral Movement** | Steal credentials (Mimikatz), pivot across internal network, escalate to admin/multisig signers | Moving from one laptop to the exchange's cold-wallet signing infrastructure |
| **5. Exfiltration & Laundering** | Transfer crypto via pre-signed transactions or forged multisig approvals → launder through mixers, DeFi, THORChain | Bybit: signers approved a *fake* transaction that actually drained 401,000 ETH |

## Their Main Attack Vectors

- **Fake Job Interviews** (most common since 2024) — "Contagious Interview" campaign targeting crypto/tech professionals
- **Supply Chain Poisoning** — 230+ malicious npm/PyPI packages mimicking popular libraries (is-buffer, eslint, redux)
- **Spear Phishing** — AI-generated, hyper-personalized lures
- **Zero-Day Exploits** — e.g., CVE-2026-68820 (Windows) used in Operation Dream Job
- **ClickFix** — tricking victims into pasting commands into their own terminal
- **Ransomware** (newer) — Medusa ransomware for double-extortion against healthcare/defense

## Key Characteristics

| Trait | Detail |
|-------|--------|
| **Patience** | Months of preparation before the final strike (Radiant Capital: 6 months; Binance: weeks) |
| **Human > Technical** | They almost never break in through code bugs — they break in through *people* |
| **Deniability** | Operates through sub-units (TraderTraitor, APT38, Andariel) to obscure attribution |
| **Adaptability** | Shifted from bank heists (2014–2017) → crypto (2018–2022) → DeFi/multisig (2023–2026) |
| **Post-Quantum C2** | Now uses ML-KEM-1024 encryption for command-and-control (Operation FlashHole, 2025) |
| **AI-assisted lures** | AI-generated social engineering content for scale |

## In One Line

They don't hack computers — they hack *people*. A fake recruiter on LinkedIn, a "coding challenge" that's actually a backdoor, and a slow, patient crawl through a company's network until they reach the one button that moves billions.   .

## Tactics & Techniques

The following are examples of techniques documented in MITRE ATT&CK's Lazarus Group profile. They are useful starting points for research, not proof that a particular sample uses them.

| Technique | ATT&CK ID | What it means |
|---|---|---|
| Spearphishing Attachment | [T1566.001](https://attack.mitre.org/techniques/T1566/001/) | Sending a targeted malicious attachment |
| Spearphishing Link | [T1566.002](https://attack.mitre.org/techniques/T1566/002/) | Sending a targeted link |
| User Execution | [T1204](https://attack.mitre.org/techniques/T1204/) | Relying on a user to open or run something |
| PowerShell | [T1059.001](https://attack.mitre.org/techniques/T1059/001/) | Using PowerShell to execute commands |
| Registry Run Keys / Startup Folder | [T1547.001](https://attack.mitre.org/techniques/T1547/001/) | Configuring automatic execution at logon |
| Scheduled Task | [T1053.005](https://attack.mitre.org/techniques/T1053/005/) | Using scheduled tasks for execution |
| System Information Discovery | [T1082](https://attack.mitre.org/techniques/T1082/) | Collecting details about a system |
| Process Discovery | [T1057](https://attack.mitre.org/techniques/T1057/) | Listing running processes |
| Web Protocols | [T1071.001](https://attack.mitre.org/techniques/T1071/001/) | Using HTTP or HTTPS for command and control |
| Exfiltration Over C2 Channel | [T1041](https://attack.mitre.org/techniques/T1041/) | Sending stolen data through a control channel |

For the full group profile and technique list, see [MITRE ATT&CK — Lazarus Group (G0032)](https://attack.mitre.org/groups/G0032/).



## Malware Analysis

In this section I reverse engineered the DTrack malware sample of Lazarus group used in October 2019 KudanKulam nuclear power plant attack.

## Static Analysis

### Some basic info about the malware from VirusTotal
<img width="1916" height="818" alt="asasa" src="https://github.com/user-attachments/assets/991e1ed6-88c9-4481-9792-a67912ce5db7" />

### VirusTotal Score on PEstudio
<img width="1189" height="851" alt="1" src="https://github.com/user-attachments/assets/8c30d725-f142-4b8f-b11b-1eb96c9763c2" />

### Floss String Analysis

<img width="433" height="444" alt="3" src="https://github.com/user-attachments/assets/c7dbf917-e774-44ab-bdd1-21e26411cea6" />

---

From the given evidence, we could identify the URLs it contacted. It possibly could be C2 domains.

- http://totalmateria.net/wp/profile2.php
- http://www.materialindia.in/wp/wp-main/galle
- http://katawaku.jp/bbs/data/theme/profile2.php

---

<img width="396" height="259" alt="2" src="https://github.com/user-attachments/assets/3d2901b6-6c48-4caf-a34b-090fec3ef1f8" />

Command execution string for persistence in scheduled tasks is identified in the strings.
- sc create ""%s"" type=own type= interact

## Ghidra Analysis to find Libraries used

<img width="234" height="219" alt="Capture" src="https://github.com/user-attachments/assets/9db9b081-d8f0-423d-a875-c45e917e731f" />

## What the Malware Can Do (Imports)

Every Windows program imports functions from system DLLs. These are the
"permissions" the malware is asking for.

| DLL | What it allows | In plain English |
|-----|---------------|-----------------|
| `kernel32.dll` | Create processes, allocate memory, read/write files, load DLLs into other processes | The master key. Without it, malware can't do anything. |
| `advapi32.dll` | Read/write registry, create Windows services, access security tokens | Hides in the registry, creates fake services, steals credentials. |
| `ole32.dll` | COM object creation, OLE automation (launch Office silently), clipboard access | Drives Word/Excel without you seeing anything. |
| `shlwapi.dll` | Path manipulation, URL parsing, shell folder access | Finds where to drop files, parses the C2 URL. |








## Detection Engineering

### YARA Rule
```
rule MyRule {
    meta:
        author = "Dishant"
        description = "Detection of DTRACK malware variants"
        date = "2026-11-10"

    strings:
        $s1 = "C:\Windows\system32\cmd.exe' /c sc create ''WBService'' type= own type= interact start= auto error= ignore binpath= 'cmd.exe /k start" nocase
        $s2 = "C:\Windows\system32\conhost.exe 0xffffffff -ForceV1" nocase
        $s3 = "sc create ''WBService'' type= own type= interact start= auto error= ignore binpath= 'cmd.exe /k start" nocase
        $s4 = "C:\Windows\System32\svchost.exe -k netsvcs -p" nocase
        $s5 = "c:\windows\system32\svchost.exe -k localservicenetworkrestricted -p -s wscsvc" nocase
    condition:
        any of them   
}   
```

## What Defenders Should Watch

Based on public reporting, useful areas to monitor include:

- Unexpected software installers or applications delivered through unsolicited messages or fake recruitment approaches.
- Unusual process chains, especially scripts or office applications launching command interpreters.
- Unexpected changes to startup entries, scheduled tasks or services.
- Unusual system and process discovery followed by outbound network connections.
- Unexpected access to credentials, browser data, cryptocurrency-related information or sensitive files.
- Attempts to disable security tools, clear logs or destroy data.

### Practical defensive steps

- Use multi-factor authentication, preferably phishing-resistant methods for important accounts.
- Keep operating systems, browsers and applications updated.
- Restrict software installation and use trusted download sources.
- Monitor process creation, persistence locations, authentication and outbound connections.
- Protect developer accounts, source-code repositories and build pipelines.
- Keep protected backups and test that they can be restored.

## Are You a Target? Risk by Organisation Scale

Lazarus doesn't pick victims by size. They pick by **value and access**.
Here's what the data shows for each tier.

### Small & Mid-Sized Businesses (< 500 employees)

| Fact | Source |
|------|--------|
| SMBs with $4M–$8M revenue became the **most targeted ransomware segment** in 2025 | Black Kite 2025 Ransomware Report |
| Medusa ransomware (Lazarus-linked) hit a **mental health nonprofit** and a **school for autistic children** — orgs with no dedicated security team | Symantec, Feb 2026 |
| 3CX supply chain attack infected small call centres, clinics, and local businesses alongside Fortune 500s — same update, same malware, no discrimination | Mandiant, 2023 |
| Lazarus uses small companies as **stepping stones** to reach larger enterprises through their supply chain | BlackFog, 2026 |
| 230+ malicious open-source packages on npm/PyPI affected **36,000 firms** in Europe, India, and Brazil (2025) — most were small dev shops | Sonatype, 2025 |

**Your risk:** You don't need to be a bank. If you run 3CX, use npm/PyPI packages, or have a developer who downloads "free" software, you're in the blast radius. The 3CX attack didn't check your employee count before infecting you.

---

### Mid-Size Enterprises (500–5,000 employees)

| Fact | Source |
|------|--------|
| Operation SyncHole (Lazarus) hit **6 organisations** in South Korea's software, IT, financial, semiconductor, and telecom sectors — all mid-size | Kaspersky, Apr 2025 |
| Operation FlashHole targeted **3 firms** in construction, software, and wire-communication manufacturing — mid-size industrial companies | Kaspersky, Oct 2026 |
| 284 victims in **India's tech sector** were hit in a single month (Dec 2024) via compromised open-source packages — most were mid-size product companies | The Register, Jan 2025 |
| ESET found Lazarus targeting **drone/UAV manufacturers** in Central and Southeastern Europe — mid-size defense contractors, not Lockheed | ESET, Oct 2025 |

**Your risk:** If you're in software, manufacturing, defense, or fintech with 500+ employees, you're a primary target. Operation Dream Job (Aug 2026) specifically targeted defense and aerospace companies in **India** via fake LinkedIn job offers using a Windows zero-day.

---

### Large Enterprises & Critical Infrastructure (5,000+ employees)

| Fact | Source |
|------|--------|
| **Bybit** (Dubai crypto exchange): $1.5B stolen — largest crypto heist in history | FBI, Feb 2025 |
| **3CX's own customer base** included American Express, BMW, Honda, IKEA, Pepsi, Toyota — all infected via the same update | 3CX, 2023 |
| **2 critical infrastructure organisations** in the energy sector (US + Europe) were hit by the X_TRADER/3CX supply chain attack | Symantec, Apr 2023 |
| **Kudankulam Nuclear Power Plant** (India) was breached by Lazarus via DTrack in 2019 | Kaspersky, 2022 |
| **Upbit** (South Korean exchange): $30M stolen (Nov 2025).  **KelpDAO + Drift**: ~$500M combined (Apr 2026) | NCC Group / TRM Labs |
| 2025 total: Lazarus stole **$2.02B** in crypto — ~60% of all crypto stolen globally that year | Chainalysis, 2025 |

**Your risk:** If you're a bank, exchange, energy company, or defense contractor, you're not just a target — you're the **prize**. But the attack vector is the same: one employee, one bad update, one fake job email. 

---

### The Bottom Line

> "Lazarus Group stole **$6.75 billion** in cryptocurrency between 2019 and 2025.
> They don't need to hack your data center. They need **one employee**
> to install a software update, click a job offer, or run a `pip install`.
>
> The 3CX attack proved it: a single employee's laptop, compromised
> through a trading app, gave them access to 600,000 downstream
> organisations — from Fortune 500s to a local call centre in Italy.
>

| If you are... | Your specific risk |
|---------------|-------------------|
| A startup / SMB using 3CX, npm, PyPI | Supply chain infection via trusted software update |
| A mid-size software or defense company in India | Operation Dream Job (fake job offers + zero-day) |
| A fintech or crypto company | Direct targeting for financial theft ($1.5B from Bybit) |
| A hospital, lab, or nonprofit | Medusa ransomware ($260K average demand) |
| A large enterprise with remote workers | ClickFix / Contagious Interview (fake meetings, credential theft) |
| Anyone with a developer on the team | 230+ trojanized packages on npm/PyPI (2025) |   

## Conclusion

For this investigation, I ran multiple malware samples associated with Lazarus. These samples are written way too sophisticated and neatly. They don't create noise, no warning, not much resource usage.
All they do is sit quietly in your system and do their job. The moment you run their .exe for installing the Lazarus IDE for coding challenge, the moment you open their fake job application document,
is the moment it executes on your computer, disables your security mechanisms quietly, dresses itself as a system process which does not look suspicious like svchost.exe , spawns children processes in backround, that could be notepad.exe
or cmd.exe, who knows. And then it adds itself to startup or scheduled tasks for persistence, so whenever you reboot your system or it crashes, the malware can re-run by itself. It keeps contacting it C2 servers for communication
and instructions awaited by the attacker. Their end goal is to either destroy you, or loot you or maybe both. These campaigns that look benign are stepping stone towards their goal. 

## References

1. [MITRE ATT&CK — Lazarus Group (G0032)](https://attack.mitre.org/groups/G0032/)
2. [CISA — AppleJeus: Analysis of North Korea's Cryptocurrency Malware](https://www.cisa.gov/news-events/cybersecurity-advisories/aa21-048a)
3. [FBI / CISA / U.S. Treasury — TraderTraitor advisory](https://www.ic3.gov/CSA/2022/220418.pdf)
4. [Associated members of Lazarus group](https://www.fbi.gov/wanted/cyber/park-jin-hyok)
