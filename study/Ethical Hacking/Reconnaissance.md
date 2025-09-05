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
		- [[Web Searching | Google Fu]], [[CLI Tools and Commands#^d16ff0| dig]], [[Useful Linux Commands For Pentesting#nmap| nmap]], Sublist3r, Bluto, crt.sh, etc.
		- The go to tool for network mapping of attack surfaces and external asset discovery is[OWASP AMASS](https://github.com/owasp-amass/amass)
	3. Fingerprinting
		- [[Useful Linux Commands For Pentesting#nmap| nmap]], Wappalyzer, WhatWeb, BuiltWith, NetCat
	4. Data breaches ^ce5358
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