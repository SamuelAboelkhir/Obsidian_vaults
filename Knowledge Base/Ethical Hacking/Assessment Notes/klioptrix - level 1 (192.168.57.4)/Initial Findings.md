---
tags:
- type/findings
- context/target/klioptrix/level/1
- context/initial
- context/phase/reconnaissance
---

### Gathered Information
- [[Common ports and protocols#9. HTTP and HTTPS - Hypertext Transfer Protocol|80/443]] on IP 192.168.57.4
- Found a default webpage - Apache - PHP
- Information disclosure
	- 404 not found page
	- Server headers disclose version information
- Interesting [[Useful Linux Commands For Pentesting#nmap| nmap]] findings
	- mod_ssl/2.8.4 - mod_ssl 2.8.7 and lower are vulnerable to a remote buffer overflow which may allow a remote shell.
	- 80/tcp    open  http        Apache httpd 1.3.20 ((Unix)  (Red-Hat/Linux) mod_ssl/2.8.4 OpenSSL/0.9.6b)
- [**Webalizer Version 2.01**](http://www.mrunix.net/webalizer/)
	- http://192.168.57.4/usage/usage_202508.html
- SMB
	- Unix (Samba 2.2.1a)
- 22/tcp    open  ssh         OpenSSH 2.9p2 (protocol 1.99)