# Pastebin: The Internet's hidden dumpsite
## AUTHOR: DISHANT 

### What really gets posted on Pastebin??

I always thought pastebin as a place for developers, shared code snippets, some urgent note downs, sharing resources and stuff like that.
This platform has been existing since 2002.Game build data, error logs, source code, links all of these stuff can be shared on pastebin, consider it an internet clipboard.
### Background

Internet threats still existed at time, and hackers had got their campaigns running. At that time, the cybersecurity posture and awareness was not good enough amongst people and organisations. Hackers did not require specialized hacking skills, they could pwn a system with just basic recon and computer basics.

* A running ftp server with anonymous access
* A poorly confugured telnet service.
* Easily guessed admin panel password
* Weak public facing applications 
* Plain text credential storage
* And many more

### Why Pastebin is used by hackers??

A small group of computer geeks could just get into systems of organisation with some pwning knowledge.
The motive of these hackers varied at a large scale. Some wanted to exfiltrate the data to sell it on darkweb, some wanted to hold their data for ransom, some wanted to mess with organisations for fame, some just wanted to test their skills.
Just like Jonathan James, the 15 year old kid who hacked into Pentagon in 1999.He found systems running with default configurations, weak administrative passwords, or unpatched software flaws that allowed him remote login capabilities.

Hackers who tried selling the data on different forums, be it surface web or darkweb, and failed, they chose to dump the stolen data on surface web. Some directly pasted the data into pastebin. This contained:

* IP addresses of organisations
* Leaked email IDs, addresses, phone number, credentials, name.
* List of target organisations to attack.
* Payloads crafted specifically to attack the public facing websites, like Command Injection, SQLi.
* Compromised targets and their details.
* Entire database schema along with credentials.


 Whereas some created files, folders, zips and uploaded them into drives and then pasted their public link into pastebin. The folders can contain: 

* IOC's like URL, executable(malwares most of the time), sometimes hashes too.
* Leaked documents
* Leaked photos and videos

As per my research, most dumping originated from around 2012-2019, later on the data dumping almost stopped abruptly. And now for current times, there is not much record for data dumping.

### How did I found all of this exposed dumped data?
The easiest and simplest answer is **Google Dorking**. It is basically a google browser search technique that helps us to filter out our needs with some key words. It makes our searching efficient.
I obviously cannot show exact dorking I used, it was just try and see the results with different terms. Try words like leaked, ransomware, malware, credentials, passwords, hacked etc.

### Now let us explore some of my findings on Pastebin
### Experian Breach
<img width="1420" height="532" alt="Screenshot From 2026-08-02 18-58-59-blurred" src="https://github.com/user-attachments/assets/5b0ab92c-20cb-4293-8d1a-ea402d43290f" />
 A major 2015 data breach that exposed the personal data of 15 million T-Mobile customers stored on Experian servers.
 Even if you find the redacted URL, no use, because most of the URLs are dead long ago.

 ### UBER FRANCE BREACH

<img width="1385" height="788" alt="5" src="https://github.com/user-attachments/assets/62f95b25-cd4f-4e06-9430-51e40a909a36" />

#### Now lets come onto malware analysis of ransomware I found in dump
<img width="751" height="355" alt="10" src="https://github.com/user-attachments/assets/93cf4a1b-f4f1-4325-8cbc-742d3b7be4a5" />


#### WannaCry was a massive global cyberattack that started on ⁠May 12, 2017, hitting over 200,000 computers in 150 countries by locking files and demanding $300 in Bitcoin.
#### The original Windows WannaCry ransomware does not run on Android, but copycats like WannaLocker and modified SLocker variants have targeted Android devices.
<img width="1869" height="960" alt="8" src="https://github.com/user-attachments/assets/c185ed6e-53ab-4596-bacd-1797027dba3a" />

#### The virus total score says it all to prove it suspicious. But we will further dive into the analysis using Hybrid Analysis platform.

<img width="1363" height="668" alt="9" src="https://github.com/user-attachments/assets/bec93157-9e1d-4fb4-8736-3a01ea399406" />


#### Taking a look at MITRE ATT&CK mapping
<img width="1687" height="659" alt="Screenshot From 2026-08-02 15-34-58" src="https://github.com/user-attachments/assets/5d834e3e-ea5e-4a3f-827e-1b731dc0eca5" />
The MITRE ATT&CK mapping do prove that obfuscation of the malware code was done in order to bypass basic security checks and conceal the payload.
Since there was a potential URL that was connected to a C2 server for communication, the ransomware definitely did many other things other than encrypting system files.

## THREAT INTEL VERDICT

**Pastebin** has been very popular amongst hackers to dump all their stolen stuff, also to spread threats. None of these groups are that big or sophisticated to be called as APTs(Advanced Persistent Threat).
To protect the organizations in a proactive way, threat hunters need to actively monitor these pasting websites for data breaches and IOCs (Indicator of Compromise). Most of the data being dumped is quite old and outdated.
But still as per my research, many leaked credentials were still active and unchanged. So other than traditional threat hunting, this addition in threat hunting methodology can be greatly helpful.




