---
tags:
  - EH
MOC: Knowledge Base
---

#### 1.  [[EH Reconnaissance]]
1. [[EH Useful Linux Commands For Pentesting#nmap|nmap]] scan to detect possibly vulnerable ports 
	1. Can give us ports like 139 (smb) and 80/443 (http/https)
	2. can give us some OS info
2. [[EH Hacking Tools#nikto|nikto]]
	1. Can give us web vulnerabilities
#### 2. [[EH Enumeration|Enumerate]] 
1. Enumerate your findings by first trying to connect to the target on the browser
	1. Http/Https
		1. http://target
		2. https://target
		3. [[EH Hacking Tools#dirbuster|Dirbuster]] with a directory list wordlist
2.  [[NET Common ports and protocols#13. SMB - Server Message Block|SMB]]
	1. metasploit
		1. search smb for available smb tools
		2. Use auxiliary scanner to enumerate host smb version
#### 3. [[EH Research]] 
1. Find exploits for the detected vulnerabilities
2. https://www.exploit-db.com
3. searchsploit
4. CVE details for vulnerability severity check
5. Vulnerability scan with [[EH Hacking Tools#nessus|nessus]]
6. Can give us various possible vulnerabilities with their severity, reason they are vulnerabilities as well as solutions to fix them
#### 4. [[EH Exploitation|Exploit]] 
1. [[EH Hacking Tools#Metasploit|metasploit]]
	1. The culmination of all the enumeration and research
	2. Pick the correct tool based on gathered info
#### 5. Post Exploitation
- Gathered information after managing to get in
- Undetected malicious activity
	- Whether or not the client security was able to detect you