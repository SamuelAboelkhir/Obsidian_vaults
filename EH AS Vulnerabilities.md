---
tags:
- klioptrix/level-1
- Assessment
- Findings/Vulnerabilities
- EH
MOC: Knowledge Base
---
[[_0000 Home|Home]] | [[_0001 Knowledge Base MOC|Back to Knowledge MOC]] | [[EH AS Klioptrix - level 1 index|Back to index]]
- [[NET Common ports and protocols#9. HTTP and HTTPS - Hypertext Transfer Protocol|80/443]] - Potentially vulnerable to OpenLuck (https://www.exploit-db.com/exploits/764), https://github.com/heltonWernik/OpenLuck
	- Penetrated with OpenLuck #context/phase/exploitation
	- Couldn't connect to the internet and was limited to the apache user
- [[NET Common ports and protocols#13. SMB - Server Message Block|139]] - Potentially vulnerable to trans2open (https://www.rapid7.com/db/modules/exploit/linux/samba/trans2open/) , https://www.exploit-db.com/exploits/7, https://www.exploit-db.com/exploits/10
	- Penetrated with metasploit [[EH Exploitation#Non-Staged payloads|non-staged]] [[EH Exploitation#Netcat Bind Shell|bind shell]] #context/phase/exploitation
	- Got privilege escalation to root