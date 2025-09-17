---
tags:
- EH
MOC: Knowledge Base
---
[[_0000 Home|Home]] | [[_0001 Knowledge Base MOC|Back to Knowledge MOC]] | [[EH Ethical Hacking index|Back to index]]
After finding potential vulnerabilities during enumeration, you want to start researching possible exploits
- A good way to start is with google
	- Copy the name of a tech that you suspect to be exploitable and add "exploit" to it in the search field
	- Google will start providing you with exploits from:
		- https://www.exploit-db.com
		- https://www.rapid7.com
		- And even some github links
- Another way to research if no internet access is available is via the CLI command `searchsploit` which is filled and updated with the exploits from exploit-db and is update as you update the database
	- Searchsploit doesn't like it when you're too specific  as it does exact string matching in its search
	- A search like `searchsploit mod ssl 2` will return a list of exploits with OpenSSL versions, and their path in `/usr/share/exploitdb/`