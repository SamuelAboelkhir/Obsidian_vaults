---
tags:
- CYB
- CYBER
MOC: Cybersecurity
---
[[_0000 Home|Home]] | [[_0005 Cybersecurity MOC|Back to Cybersecurity MOC]] | [[CYB Cybersecurity index|Back to index]]
# Types of malware
#### Spyware
- Designed to spy on everything you do.
- Can monitor your online activity
- Can log every key you press
- Can capture "almost" any of your data
- Accomplishes this by modifying the security settings on the device
- Can bundle itself with legit software or Trojan horses
#### Adware
- Designed to automatically deliver ADs to a user
- Commonly found on web browsers
- Usually come with spyware
- Often installed with versions of software
#### Backdoor
- Designed to bypass normal authentication procedures and grant unauthorized access to a system
- Hackers use it to gain remote access to resources within an app, and issue remote system commands
- Works in the background and is difficult to detect
#### Ransomware
- Designed to hold a computer or data captive until payment is made
- Usually works by encrypting data
- Can take advantage of specific system vulnerabilities to lock it down
- Often spread through phishing emails or software vulnerabilities
#### Scareware
- Uses scare tactics to trick you into taking a specific action
- Consists of OS style windows that warn you the system is at risk
- Asks you to use specific programs to return to normal
- Using the program is how you get compromised
#### Rootkit
- Designed to modify an OS to create a backdoor
- Takes advantage of software vulnerabilities to gain access (privilege escalation)
- Can modify system forensics and monitoring tools to mask themselves
- Usually the affected OS has to be reinstalled
#### Virus
- A type of program that replicates upon execution and attaches to other executables by inserting it's own code 
- Require end user interaction to activate
- Can be written to act on a specific date/time
- Can be harmless (display a funny image) or destructive (delete/modify files)
- Can be programmed to mutate and avoid detection
- Usually spread via USB drives, optical disks, network shares or email
#### Trojan horse
- Carries malicious operations by masking its true intent
- Appears legit
- Exploits your user privileges
- Usually found in image and audio files or games
- Can't self-replicate
- Act as decoy to sneak in the actual malicious software
#### Worms
- Can replicate itself and spread from one computer to another
- Doesn't require a host program to run
- Don't require user interaction past the initial infection
- All worms exploit vulnerabilities, replicate, and contain payloads to cause damage to a system or network
- Responsible for some of the most devastating attacks
- Code Red is a worm that managed to infect 300,000+ servers in 19 hours
# Symptoms of malware
- Increase CPU usage
- Freezes and crashes
- Unexplainable network connection problems
- Modified/deleted files
- Presence of unknown files/programs/desktop icons
- Unknown running processes
- Programs terminating or reconfiguring themselves
- Emails being sent without your knowledge
# Infiltration methods
#### Social engineering
- The act of manipulation of people into performing actions or divulging confidential information.
- **Pretexting**
	- Attacker calls an individual and lies to gain access to data
- **Tailgating**
	- Attacker follows an authorized person into a secure location
- **Something for something (quid pro quo)**
	- Attacker requests information in exchange for something
#### DoS - Denial of service
- Types:
	- Overwhelming quantity of traffic:
		- A network, host or application is sent a ton of data at a rate it can't handle causing it the transmission or response to slow down, or the device/service to crash.
	- Maliciously formatted packets:
		- A packet (collection of data that flows between a source and a receiver. Check the [[NET Common ports and protocols|OSI Model]]) is maliciously formatted and sent to the receiver who wont be able to handle it. The packet is maliciously formatted when it contains errors or improperly formatted packets that are unidentifiable by applications. This usually causes the receiver to run slowely or crash.
#### DDoS - Distributed DoS
- Similar to DoS but originates from multiple coordinated sources
- Attackers build networks (botnet) of infected hosts called zombies that are controlled by handler systems.
- Zombie computers constantly scan for more hosts to infect.
- The attacker can instruct the botnet of zombies to carry out a DDoS whenever he wants.
#### Botnet
- A bot computer is typically infected by visiting an unsafe website or opening an infected email attachment or infected media file. A botnet is a group of bots, connected through the Internet, that can be controlled by a malicious individual or group. It can have tens of thousands, or even hundreds of thousands, of bots that are typically controlled through a command and control server.
- These bots can be activated to distribute malware, launch DDoS attacks, distribute spam email, or execute brute-force password attacks. Cybercriminals will often rent out botnets to third parties for nefarious purposes.
- Many organizations. like Cisco, force network activities through botnet traffic filters to identify any botnet locations.
# On-path attacks
- This is when an attacker intercepts or modifies communications between two devices, such as a browser and server to collect info or impersonate one of them.
- This attack comes in two flavors:
#### MITM - Man in the middle
- The attacker takes control of the user's device discreetly and intercepts the user's information before it reaches its intended destination
- Many types of malware possess MITM capabilities
- Primarily used to steal financial data
#### MITMO - Man in the mobile
- A variation of MITM that targets mobile devices.
- Infected devices are instructed to exfiltrate sensitive user info and send it to the attacker
- `ZeuS` is a malware package that has MITMO capabilities allowing attackers to capture two step verification SMS messages quietly
# SEO poisoning
- The act of using SEO and popular search terms to push malicious sites to the top of the search results.
# Password attacks
#### Password spraying
- See also [[EH Exploitation#Password spraying|Password spraying]]
- This technique attempts to gain access to a system by ‘spraying’ a few commonly used passwords across a large number of accounts. For example, a cybercriminal uses 'Password123' with many usernames before trying again with a second commonly-used password, such as ‘qwerty.’
- This technique allows the perpetrator to remain undetected as they avoid frequent account lockouts.
#### Dictionary attacks
- A hacker systematically tries every word in a dictionary or a list of commonly used words as a password in an attempt to break into a password-protected account.
#### Brute-force attacks
- The simplest and most commonly used way of gaining access to a password-protected site, brute-force attacks see an attacker using all possible combinations of letters, numbers and symbols in the password space until they get it right.
#### Rainbow attacks
- See also [[EH Hacking Tools#hashcat|hash cracking with hashcat]]
- Passwords in a computer system are not stored as plain text, but as hashed values (numerical values that uniquely identify data). A rainbow table is a large dictionary of precomputed hashes and the passwords from which they were calculated.
- Unlike a brute-force attack that has to calculate each hash, a rainbow attack compares the hash of a password with those stored in the rainbow table. When an attacker finds a match, they identify the password used to create the hash.
#### Traffic interception
- Plain text or unencrypted passwords can be easily read by other humans and machines by intercepting communications.
- If you store a password in clear, readable text, anyone who has access to your account or device, whether authorized or unauthorized, can read it.
