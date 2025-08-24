### Five stages of ethical hacking
1. Reconnaissance
	- Active
	- Passive
2. Scanning and enumeration
	- Nmap, Nessus, Nikto, etc
3. Gaining Access ("Exploitation")
4. Maintaining Access
5. Covering Tracks

### Recon - Passive(OSINT)
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
		- Google Fu, dig, Nmap, Sublist3r, Bluto, crt.sh, etc.
	3. Fingerprinting
		- Nmap, Wappalyzer, WhatWeb, BuiltWith, NetCat
	4. Data breaches
		- HaveIBeenPwned, Breach-Parse, WeLeakInfo


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