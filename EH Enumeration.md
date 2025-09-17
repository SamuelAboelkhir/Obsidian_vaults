---
tags:
- EH
MOC: Knowledge Base
---
[[_0000 Home|Home]] | [[_0001 Knowledge Base MOC|Back to Knowledge MOC]] | [[EH Ethical Hacking index|Back to index]]
- Enumeration revolves around perusing leads, and exploring all possible angles of attack
- A possible flow is something like: 
	- Using [[EH Useful Linux Commands For Pentesting#nmap|nmap]] to find exploitable ports 
	- Then [[EH Hacking Tools#^b7c0a7|nikto]] to identify possible weaknesses in web applications 
	- Followed by a [[EH Hacking Tools#^b7c0a7|dirbuster]] scan with a wordlist file on one of the exploitable ports
##### smbclient
- **Refer To:**
		- [[EH Hacking Tools#smbclient]]
- Great for trying to enumerate when an SMB port is discovered
##### SSH
- **Refer To:**
		- [[NET Common ports and protocols#2. SSH - Secure Shell]]
- The reason to attempt SSH enumeration despite it being so secure is that you may be shown a banner telling you the SSH version and who made it, which info that can be further enumerated
- For older machines when you try to ssh you may be told no matching key exchange method is found
	- In this case, try `ssh 192.168.57.4 -oKexAlgorithms=+[the machine's offer]` as the machine will be offering a specific exchange method
	- You can then follow this up with `ssh 192.168.57.4 -oKexAlgorithms=+[the machine's offer] -c [offered cipher]` when you get a 2nd error telling you that no matching cipher was found
