---
tags: 
- LI
- TECH
MOC: Technology
---

[[_0000 Home|Home]] | [[_0001 Technology MOC|Back to Technology MOC]] | [[TECH Linux index|Back to index]]
# Links: 
- [[#Various command line tools]]
- [[#File operations]]
- [[#System management and monitoring]]
- [[#Networking commands and tools]]
- [[#System protection]]
- [[#System navigation]]
- [[#Photos and video]]
- [[#Ghostty]]
- [[#Gaming]]
- [[#Commands useful in pentesting]]
### Various command line tools
#### Back to top: [[#Links]]
- `apropos` : helps when you can't remember a specific command name.
- `dpkg --list` : shows all the executables that you have.
- `exa` : better ls.
- `batcat`: better cat.
- `compgen -b` : shows all built-in terminal commands.
- `compgen -c` : shows all command-line tools available in the system's PATH.
- (the one provided in this case)
- `ls -lt /var/lib/dpkg/info/*.list` : shows a list of installed packages 
- sorted by date.
- `curl cht.sh` : a command line cheat sheet for multiple programming languages.
- `sudo su` : switches a session to the root user. Use exit to go back.
- `su - username`: switches back to a user.
- vipe: pipe stdin into the text editor, and save quit to pipe the output to stdout.
- `sudo update-alternatives --config x-terminal-emulator`: Change default terminal.
- `chsh -s /bin/${shell}`: Replace ${shell} with the shell you want to change the default shell.
---
### File operations
- `cat`: prints file content to stdout
	- `batcat`: better cat that adds scrolling among other features
- `ls`: shows the contents of a folder
	- `exa`: better ls that adds icons and color
- `wc`: word count
- `uniq`: finds unique occurrences of lines in the file 
- `cut`: extracts specific columns from a file
- `diff`: compares the contents of files
	- `colordiff`: diff but with color highlighting
- `sort`: sorts the contents of the file
- `sudo rsync -avxHAXS --progress /mnt/old_ubuntu24/home/ /mnt/new_home/`
	- `a` : archive mode (preserves everything)
	- `v `: verbose (show files being copied)  
	- `x` : don't cross filesystem boundaries
	- `H` : preserve hard links
	- `A` : preserve ACLs
	- `X` : preserve extended attributes
	- `S` : handle sparse files efficiently
	- `--progress` : show progress bar
- `find`: Search for files in a directory hierarchy
	- `-exec` Allows you to execute a command on each found file
		- the syntax would be `find [path] [conditions] -exec [command] {} \;`
		- `{}` is a placeholder for the current found filename
		- `\;` Marks the end of the command. Replace with `';'` in zsh
```bash
Examples:

find . -type f -name "EH*" -exec sed -n '/MOC: Technology/p' {} ';'
- find . 
	# search in current directory
- -type f 
	# only files (not directories)
- -name "EH*" 
	# files starting with "EH"
- -exec sed -n '/MOC: Technology/p' {} ';' 
	# run `sed` on each file
- sed -n 
	# suppress default output
- '/MOC: Technology/p' 
	# print lines containing "MOC: Technology"
- {}
	# current filename
- ';'
	# end the exec command
find . -type f -name "EH*" -exec sed -i 's/MOC: Technology/MOC: Cybersecurity/g' {} ';'
- sed -i
	# edit files in-place (saves changes)
- s/old/new/g
	# substitute old with new globally
You can also end with `+` instead of `';'` to process multiple files at once
You can add `-ok` instead of `-exec` to ask for confirmation before every operation
You can pass multiple commands with multiple `-exec`
You can create a complex command with `sh -c`
find . -name "*.txt" -exec sh -c 'echo "Processing: $1"; wc -l "$1"' _ {} \;
```
- [[TECH xargs|xargs]]: Reads items from standard input and executes commands with those items as arguments.
- [[TECH Awk Command Cheat Sheet & Quick Reference|awk]]
- [[TECH Sed Command Cheat Sheet & Quick Reference|sed]]
- `eval`: eval is a built-in Linux command that executes arguments as a shell command. It combines arguments into a single string, uses it as input to the shell, and executes the commands
- 
- `ln`: Creates links between files
	- `ln -s /home/file1 /home/Documents/link-to-file1`
- #### Back to top: [[#Links]]
---
### System management and monitoring

#### Back to top: [[#Links]]
- `sudo sysctl -w fs.inotify.max_user_watches=131070` : increase the limit of file watchers.
- `ncdu` : a tool that shows you the distribution of disk space.
- `free -h` : shows memory statistics in a human readable format.
- `swapon --show` : shows the available swapfiles and their usages.
-  `more /proc/sys/vm/swappiness` : shows the swappiness statistic of the system.
- `sudo dd if=dev/zero of=/swapfile2 bs=1M count=2048 status=progress; sudo chmod 600 /swapfile2; sudo mkswap /swapfile2; sudo swapon /swapfile2` : a command that starts with dd, a command for reading, writing, and converting file data. The command contains the following parameters:

> if=/dev/zero is the input file. The /dev/zero file is a special file that returns as many null characters as a read operation requests.
of=/swapfile is the output swap storage file. The common practice is to place the file in the root directory.
The bs parameter is the block size.
The count parameter determines how many blocks to copy.

> Followed by chmod 600 to give the swapfile read and write permissions, then mkswap to convert the file size to be reserved for swap, swapon then activates the swapfile (it's best to add the swapfile in /etc/fstab for it to become persistent between restarts)

- `lshw`: Shows hardware information.
- `duf`: Shows disk usage.
- `glances`: Monitoring swiss knife which is extensible, and has alarms support.
- `nvtop`: Specific nvidia GPU monitor.
- `btop`: Monitor for CPU, DISK, RAM, Network that's fast and pretty.
- `uname`: Prints certain system information
- `passwd`: sets system password
- `groups`: shows available groups ^2ddd0f
- `getent` : Displays entries from databases supported by the NSS switch libraries ^870e82
- `getent passwd` or `cat /etc/passwd`: shows all available users ^95dc24
- `getent group` or `cat /etc/group`: shows all available groups and their members
- `sudo getent shadow or sudo cat /etc/shadow`: shows you the password hashes of users that have passwords ^46071d
- `ps aux` lists all running processes
	- ps = Process Status command
	- a = Show processes for ALL users (not just current user)
	- u = Show in USER-oriented format (detailed info)
	- x = Show processes WITHOUT controlling terminal (background processes)
- Top CPU users
	- ps aux --sort=-%cpu | head -10
 - Top memory users  
	- ps aux --sort=-%mem | head -10
 - Processes using most CPU right now
	- top -o %CPU
- `jobs -l` shows background jobs in current shell with process ID
-  Information about specific process (replace 1234 with actual PID)
	- cat /proc/1234/status
	- cat /proc/1234/cmdline
	- ls -la /proc/1234/
- `sudo -l`: shows you available sudo privileges ^424c38
- `sudo cat /etc/sudoers`: shows you which users and groups have which sudo permissions ^85415c
- `systemctl` runs services at boot ^87cbfa
- `service`, example: Web servers ^484cec
	1. Apache: Allows you to start a server and host software on it
		1. Start the server with`sudo service apache2 start`
		2. Type your IP address into the browser. The server will be running on port 80
		3. Stop the service with `sudo service apache2 stop`
	2. You can also start servers with python
		1. `python3 -m http.server [port number]` ^b96608
		2. The webserver starts within the folder you're currently inside
- `showmount`: Shows mounted directories
	- `-e`: Shows the mounts on a server
- `mount`: Mount directories to your system
	- Can mount remote directories
	- `-t`: indicates the filesystem type
- [[TECH SSH |ssh]]
---
### Networking commands and tools

#### Back to top: [[#Links]]
- `arp`: shows the device's arp table. ^225c89
- `nmap -sn 192.168.1.1/24`: shows all the IPs in the specified range and subnet mask as well as their open ports. Use --verbose on all commands for more details. ^2b1c6e
- `nmcli`: CLI network manager.
- `mtr`: shows both ping and traceroute to a specific IP.
- `dig`: does DNS lookups and reverse DNS with the -x flag ^d16ff0
- `ifconfig`/`ip`: both show IPs and network interface information. ^a631d5
- `netstat`: for network statistics.
- `host`: shows info about a host.
- `hostname`: shows the hostname. Use with -I to see the IPs.
- `iftop`: a network monitoring tool.
- `curl`/`wget`: tools for data transfer/downloading files via the terminal.
- `Lynx` : a simple terminal web browser.
- `lsof -i [port]`: Shows the process that owns or is using the port.
- `termshark`: CLI tshark.
- The following commands can be used to flush and replace the IP address assigned by DHCP, as long as IP assignment is no longer automatic 
```bash
sudo ip addr flush dev eth0
sudo ip addr add 192.168.57.10/24 dev eth0
sudo ip route add default via 192.168.57.1
```

- `ps aux | grep -E "(dhcp|network|wpa|nm-)"`: finds network related processes
- To find processes using specific ports
	- `sudo netstat -tulpn | grep :80`

- `sudo dhcpcd eth0` lets dhcpd run on your interface and give it an IP address

---
### System protection

#### Back to top: [[#Links]]
- `ufw`: local firewall.
- `clamAV`: anti-virus.
---
### System navigation
- `locate`: provide it with a file name and it will give you its location
	- It traverses an index of the file system that it saves in a DB
	- Requires updating that db via `sudo updatedb`
#### Back to top: [[#Links]]
- `Ranger` : file management tool.
- `rofi` : window switcher that can also run commands and browse files.
---
### Photos and video

#### Back to top: [[#Links]]
- `mpv`: command line media player.
- `timg`: command line image and video player.
- `ffmpeg`: media convertor
---
### Ghostty

#### Back to top: [[#Links]]
- `ghostty +show-config --default --docs`: prints ghostty docs to stdout (use with vipe to get it in nvim)
---
### Gaming

#### Back to top: [[#Links]]
- How to run a game with the discrete GPU :
`__NV_PRIME_RENDER_OFFLOAD=1 __GLX_VENDOR_LIBRARY_NAME=nvidia __VK_LAYER_NV_optimus=NVIDIA_only wine /home/blackdovah/ubuntu24/home/blackdovah/Games/Exanima.v0.9.0.5/Exanima.v0.9.0.5/Exanima.exe`
- # Always purge old driver first
	- sudo apt purge "nvidia-driver-*" "nvidia-dkms-*" sudo apt autoremove --purge 
	 ###### Then install new one 
	- sudo apt install nvidia-driver-535 sudo reboot
---
#### Back to top: [[#Links]]
# Commands useful in pentesting
### Finding Users Data
- [[TECH CLI Tools and Commands#^870e82|Getent]] with group/passwd/shadow(with sudo).  
	-  [[TECH CLI Tools and Commands#^2ddd0f|Group]]  will show you all members of a group
	-  [[TECH CLI Tools and Commands#^95dc24|Passwd]] will show you all users
	-  [[TECH CLI Tools and Commands#^46071d|Shadow]] will show you the password hashes of users that actually have passwords
-  [[TECH CLI Tools and Commands#^85415c|sudoers]] can get you info about which users and groups have which permissions
- To see sudo privileges [[TECH CLI Tools and Commands#^424c38|sudo -l]]
### Privileges/permissions
-  Privileges on Linux follow the structure `drwxrwxrwx`
	- `d`: directory. If it's empty `-` it means file, and if it's `l` it means a symlink
	- `rwx`: read, write, execute
		- first rwx = current user/owner
		- second rwx = group
		- third rwx = other users
- This line shown by `ls -la` is normally followed by two columns | user group | showing the user name and the group name that have access to the file with rwx defining their privileges being read, write, and execute
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
- [[TECH CLI Tools and Commands#^a631d5|ifconfig]]
- [[TECH CLI Tools and Commands#^225c89|arp]] `-a`
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
1. [[TECH CLI Tools and Commands#^484cec|service such as webservers]]
	1. Useful in [[EH Exploitation#Privilege Escalation]] as you can have scripts on your server that you download on the target machine with `wget`
2. [[TECH CLI Tools and Commands#^87cbfa|systemctl]]
---