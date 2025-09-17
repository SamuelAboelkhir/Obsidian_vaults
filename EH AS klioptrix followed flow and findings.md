---
tags:
- klioptrix/level-1
- Assessment
- Flow
- EH
MOC: Knowledge Base
---
[[_0000 Home|Home]] | [[_0001 Knowledge Base MOC|Back to Knowledge MOC]] | [[EH AS Klioptrix - level 1 index|Back to index]]
1. [[EH Useful Linux Commands For Pentesting#nmap|nmap]] scan to detect possibly vulnerable ports 
	1. Gave us port 139 (smb) and 80/443 (http/https)
	2. Gave us some OS info
2. [[EH Enumeration|Enumerate]] findings by first trying to connect to the target on the browser
	1. Http/Https
		1. http://target
			1. Showed us a default page that included info about the OS
		2. https://target
		3. [[EH Hacking Tools#dirbuster|Dirbuster]] with a directory list wordlist
	2.  [[NET Common ports and protocols#13. SMB - Server Message Block|SMB]]
		1. metasploit
			1. search smb for available smb tools
			2. Use auxiliary scanner to enumerate host smb version
				1. Gave us the version (samba 2.2.1)
3. [[EH Research]] exploits for the detected vulnerabilities
	1. https://www.exploit-db.com
		1. Taught us about trans2open exploits for our vulnerability
	2. searchsploit
	3. CVE details for vulnerability severity check
4. Vulnerability scan with [[EH Hacking Tools#nessus|nessus]]
	1. Gave us various possible vulnerabilities with their severity, reason they are vulnerabilities as well as solutions to fix them
5. Exploit with [[EH Hacking Tools#Metasploit|metasploit]]
	1. The culmination of all the enumeration and research
	2. Pick the correct tool based on gathered info
	3. Here it was linux/samba/trans2open
	4. Attempted [[EH Exploitation#Non-Staged payloads|non-staged]] and [[EH Exploitation#Staged payloads|staged]] [[EH Exploitation#Netcat Reverse Shell|reverse]] and [[EH Exploitation#Netcat Bind Shell|bind]] shell
		1. staged bind shell worked
		2. Root access acquired