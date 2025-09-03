### Reminder: People are the weakest link in any organization
# Five stages of ethical hacking
1. Reconnaissance
	- Active
	- Passive
2. Scanning and enumeration
	- [[Useful Linux Commands Explained#nmap | nmap]], [[Hacking Tools#^687ee2|  nessus]], [[Hacking Tools#^b7c0a7 | nikto]], etc
3. Gaining Access ("Exploitation")
4. Maintaining Access
5. Covering Tracks

# Recon  

### Passive(OSINT)
1. Physical/social recon which include:
	1. Location information
		1. Satellite images
		2. Drone recon
		3. Building layout (badge readers, break areas, security, fencing)
	2. Job Information
		1. Employees (name, job title, phone number, manager, etc.)
		2. Pictures (badge photos, desk photos, computer, photos, etc.)
2. Web/Host
	1. Target validation
		- WHOIS, nslookup, dnsrecon
	2. Finding subdomains
		- [[Web Searching | Google Fu]], [[Useful Command-line Tools#^d16ff0 | dig]], [[Useful Linux Commands Explained#nmap | nmap]], Sublist3r, Bluto, crt.sh, etc.
		- The go to tool for network mapping of attack surfaces and external asset discovery is[OWASP AMASS](https://github.com/owasp-amass/amass)
	3. Fingerprinting
		- [[Useful Linux Commands Explained#nmap | nmap]], Wappalyzer, WhatWeb, BuiltWith, NetCat
	4. Data breaches
		- [HaveIBeenPwned](https://haveibeenpwned.com), [Breach-Parse](https://github.com/hmaverickadams/breach-parse), WeLeakInfo


### Identifying emails
1. Email identifying websites
	1. [hunter.io](https://hunter.io) can identify emails for you via the company name
	2. [phonebook.cz](https://phonebook.cz) find emails, domains and URLs
	3. [voilanorbert](https://www.voilanorbert.com)
	4. clearbit (chrome extension)
2. Verify found emails
	1. [verifyemailaddress.io](https://tools.emailhippo.com)
	2. [email-checker.net/validate](https://email-checker.net/validate)
	3. Try forgot my password using an email. If it works, the email is valid, and will give you a hint to an email that's tied to it where the verification code was sent.

### Validating subdomains
- [tomnomnom httprobe](https://github.com/tomnomnom/httprobe) can take a list of found subdomains and validate which of them is actually alive and usable

### Finding tech stack
1. [builtwith](https://builtwith.com/) finds the stack a website is built with
2. wappalyzer is a firefox add-on that also identifies the tech stack of the web page
3. whatweb (CLI) is a CLI tool that identifies recognizes web pages and identifies their technologies

### Bug bounty hunting websites
1. [bugcrowd](https://www.bugcrowd.com/customer/)

# Enumeration
- Enumeration revolves around perusing leads, and exploring all possible angles of attack
- A possible flow is something like: 
	- Using [[Useful Linux Commands Explained#nmap | nmap]] to find exploitable ports 
	- Then [[Hacking Tools#^b7c0a7 | nikto]] to identify possible weaknesses in web applications 
	- Followed by a [[Hacking Tools#^b7c0a7 | dirbuster]] scan with a wordlist file on one of the exploitable ports
- [[Hacking Tools#^4f8b18| smbclient]] is great for trying to enumerate when an SMB port is discovered
- SSH
	- The reason to attempt SSH enumeration despite it being so secure is that you may be shown a banner telling you the SSH version and who made it, which info that can be further enumerated
	- For older machines when you try to ssh you may be told no matching key exchange method is found
		- In this case, try `ssh 192.168.57.4 -oKexAlgorithms=+[the machine's offer]` as the machine will be offering a specific exchange method
		- You can then follow this up with `ssh 192.168.57.4 -oKexAlgorithms=+[the machine's offer] -c [offered cipher]` when you get a 2nd error telling you that no matching cipher was found

# Research
- After finding potential vulnerabilities during enumeration, you want to start researching possible exploits
- A good way to start is with google
	- Copy the name of a tech that you suspect to be exploitable and add "exploit" to it in the sarch
	- Google will start providing you with exploits from:
		- https://www.exploit-db.com
		- https://www.rapid7.com
		- And even some github links
- Another way to research if no internet access is available is via the CLI command `searchsploit` which is filled and updated with the exploits from exploit-db and is update as you update the database
	- Searchsploit doesn't like it when you're too specific  as it does exact string matching in its search
	- A search like `searchsploit mod ssl 2` will return a list of exploits with OpenSSL versions, and their path in `/usr/share/exploitdb/`


# Exploitation
### Netcat Reverse Shell
- Most common
	- You use it 95% of the time
- A shell gives you access to a machine
	- Attackbox listens - target connects
- A reverse shell is a shell that allows a victim to connect to you
	- You can have netcat active on the attackbox listening on a port, and the target connects to your IP on the same port via netcat
- Port forward or port trigger is needed to connect the target's public IP to the attackbox's private IP
### Netcat Bind Shell
- Less common
	- Mostly on external assessments
- Most useful when you need to bypass a firewall or when reverse shell isn't working
- The attackbox connects to the target
	- Attackbox connects - target listens
- We open a port on the target via an exploit, then connect to it via netcat
- No need for port forwarding since we connect the attackbox's public IP to the target's private IP
# Notes
- It's important to keep a note file of your findings during a pentest
- You should have a directory dedicated for the project where you also keep a file with the scan result of every used tool

# Hacking Tools
- For comprehensive hacking tools notes, follow this link [[Hacking Tools]]

# Assessments
