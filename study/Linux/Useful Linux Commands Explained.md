More tools are available at [[Useful Command-line Tools]]

### Finding Users Data
- [[Useful Command-line Tools#^870e82| Getent]] with group/passwd/shadow(with sudo).  
-  Group  will show you all members of a group
-  Passwd will show you all users
-  Shadow will show you the password hashes of users that actually have passwords
-  `sudo cat /etc/sudoers` can get you info about which users and groups have which permissions
- You can use `sudo -l` to see which privileges you have access to with sudo
### Privileges
-  Privileges on Linux follow the structure drwxrwxrwx
-  This line is split into d (directory) 1-rwx 2-rwx 3-rwx where each set represents a group: user/owner, group, and other users respectively
- This line shown by ls -la is normally followed by two columns | user group | showing the user name and the group name that have access to the file with rwx defining their privileges being read, write, and execute
- You can change a file's privileges with `chmod` using +rwx or -rwx to add or remove privileges
- Chmod also works with the following totals
![Chmod privileges](assets/Screenshot%20from%202025-08-24%2003-55-14.png)

### Users And Groups
- You can add a user with `sudo addUser [name]` but this is debian specific
-  You can also use `sudo useradd [name]` followed by `sudo passwd [name]`
- You switch to that user with `su [name]`
- You can add a group with `sudo groupadd [group]` then add a user with `sudo usermod -a -G [group] [user]`
### Networking
The following commands will have 2 versions. The old version of the command, and the recent version.

New:
- `ip a` shows all network interfaces
- `ip n` shows all neighboring end devices
- `ip r` shows the routing table

Old:
- [[Useful Command-line Tools#^a631d5 | ifconfig]]
- [[Useful Command-line Tools#^225c89 | arp]] `-a`
- `route`

- `ping` shows you if a machine is on the network via an ICMP request. Note that a host machine may have ICMP disabled and appear to not be connected

- `arp-scan -l` scans all devices on a network
- `netdiscover -r 192.168.57.0/24` sweeps an entire subnet showing data in a table

### nmap
- nmap runs in stealth mode by default, in which case instead of a normal TCP three-way handshake: SYN SYNACK ACK, it does SYN SYNACK RST, however, this can be picked up by decent security measures
- `nmap -T4 -p- -A` 
	- `-T` determines the speed of the process which is between 1 and 5 (5 is the fastest)
	- `-p-` says to scan all ports. Without it the top 1000 ports are scanned by default. `-p` alone can be succeeded with specific port numbers
	- `-A` is an aggressive scan, which means that it will find all possible information such as fingerprinting, OS details, etc.
- nmap has multiple uses, such as: 
	1. Host discovery. ex: `-sn` for ping sweep
	2. Scan techniques. ex: `-sS` stealth scan, `-sU` UDP scan
		- Note, it's best to (especially with UDP scan) not use -A at first, and save it until the open ports have been identified as leaving -A in an all ports scan is much slower. However, that's not needed especially if you're working on other stuff (such as OSINT) and have time to give the nmap scan to finish.

### Services
1. `service`, example: Web servers
	1. Apache: Allows you to start a server and host software on it
		1. Start the server with`sudo service apache2 start`
		2. Type your IP address into the browser. The server will be running on port 80
		3. Stop the service with `sudo service apache2 stop`
	2. You can also start servers with python
		1. `python3 -m http.server [port number]`
		2. The webserver starts within the folder you're currently inside
2. `systemctl` runs services at boot

