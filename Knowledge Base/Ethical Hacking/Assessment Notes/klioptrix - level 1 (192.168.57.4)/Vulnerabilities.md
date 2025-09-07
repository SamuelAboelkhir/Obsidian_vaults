---
tags:
- type/findings
- context/target/klioptrix/level/1
- context/vulnerability
- context/phase/enumeration
---

- [[Common ports and protocols#9. HTTP and HTTPS - Hypertext Transfer Protocol|80/443]] - Potentially vulnerable to OpenLuck (https://www.exploit-db.com/exploits/764), https://github.com/heltonWernik/OpenLuck
	- Penetrated with OpenLuck #context/phase/exploitation
	- Couldn't connect to the internet and was limited to the apache user
- [[Common ports and protocols#13. SMB - Server Message Block|139]] - Potentially vulnerable to trans2open (https://www.rapid7.com/db/modules/exploit/linux/samba/trans2open/) , https://www.exploit-db.com/exploits/7, https://www.exploit-db.com/exploits/10
	- Penetrated with metasploit [[Exploitation#Non-Staged payloads|non-staged]] [[Exploitation#Netcat Bind Shell|bind shell]] #context/phase/exploitation
	- Got privilege escalation to root