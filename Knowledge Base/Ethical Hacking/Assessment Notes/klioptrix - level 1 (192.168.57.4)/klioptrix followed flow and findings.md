---
tags:
- type/flow/findings
- context/target/klioptrix/level/1
---

1. [[Useful Linux Commands For Pentesting#nmap|nmap]] scan to detect possibly vulnerable ports 
	1. Gave us port 139 (smb) and 80/443 (http/https)
	2. Gave us some OS info
2. [[Enumeration|Enumerate]] findings by first trying to connect to the target on the browser
	1. Http/Https
		1. http://target
			1. Showed us a default page that included info about the OS
		2. https://target
		3. [[Hacking Tools#dirbuster|Dirbuster]] with a directory list wordlist
	2.  [[Common ports and protocols#13. SMB - Server Message Block|SMB]]
		1. metasploit
			1. search smb for available smb tools
			2. Use auxiliary scanner to enumerate host smb version
				1. Gave us the version (samba 2.2.1)
3. [[Research]] exploits for the detected vulnerabilities
	1. https://www.exploit-db.com
		1. Taught us about trans2open exploits for our vulnerability
	2. searchsploit
	3. CVE details for vulnerability severity check
4. Vulnerability scan with [[Hacking Tools#nessus|nessus]]
	1. Gave us various possible vulnerabilities with their severity, reason they are vulnerabilities as well as solutions to fix them
5. Exploit with [[Hacking Tools#Metasploit|metasploit]]
	1. The culmination of all the enumeration and research
	2. Pick the correct tool based on gathered info
	3. Here it was linux/samba/trans2open
	4. Attempted [[Exploitation#Non-Staged payloads|non-staged]] and [[Exploitation#Staged payloads|staged]] [[Exploitation#Netcat Reverse Shell|reverse]] and [[Exploitation#Netcat Bind Shell|bind]] shell
		1. staged bind shell worked
		2. Root access acquired