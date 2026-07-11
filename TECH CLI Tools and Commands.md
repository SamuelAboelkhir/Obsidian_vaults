---
tags: 
- CLI
- LI
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
- [[#Photos, audio and video]]
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
	- `compgen -c | grep '^xdg-'`: Gets a list of all the XDG commands
	- xdg-* commands are part of the freedesktop.org “XDG” tools, which are meant to be desktop-agnostic.
		- Works on GNOME, KDE, XFCE, LXDE, etc.
		- Handles things like opening files, setting default apps, opening URLs, launching the preferred browser, etc.
		- It’s basically Linux’s cross-desktop utility layer.
- (the one provided in this case)
- `ls -lt /var/lib/dpkg/info/*.list` : shows a list of installed packages 
- sorted by date.
- `curl cht.sh` : a command line cheat sheet for multiple programming languages.
- `sudo su` : switches a session to the root user. Use exit to go back.
- `su - username`: switches back to a user.
- vipe: pipe stdin into the text editor, and save quit to pipe the output to stdout.
- `sudo update-alternatives --config x-terminal-emulator`: Change default terminal.
- `chsh -s /bin/${shell}`: Replace ${shell} with the shell you want to change the default shell.
- `tinyxxd`: Does a pretty hexdump
- `hexdump`: Does a normal hexdump (the hex is split into groups of 4 and the first 2 and last 2 digits in each group are swapped compared to tinyxxd)
- `expr`: A command that evaluates expressions
- `trans`: A shell dictionary and translator, which is basically a google translate wrapper
- `ldd`: list dynamic dependencies. It lists the dependencies of a binary file, and whether or not the binary file can locate them
---
### File operations
#### Back to top: [[#Links]]
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
	- `fd`: Better find
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
- `ln`: Creates links between files
	- `ln -s /home/file1 /home/Documents/link-to-file1`
	- `ln` is the link command and `-s` stands for symbolic
	- The target can be either an absolute path or a relative one, where with the absolute path, the link is maintained as long as the target's location doesn't change, but with a relative path, the relative positions of the target and link paths is what needs to be preserved, otherwise they can both be moved together
	- As an example, if I'm in the following directory 
	- `~/Work and Education/bootdev/Linux/worldbanc/public`
	- I can create the following symlink
	- `❯ ln -s ../investments/tbills.txt products/credit_cards/tbills.txt`
	- Where the target is written relative to the link (as if the CWD was the credit_cards directory)
	- A link can then be seen with `ls -l`
- `pandoc`: General markup converter with multiple different format options
- `glow`: TUI markup renderer
- `evtest`: A tools that captures a device's inputs
	- Used with `sudo` as it needs to scan a root file for the list of devices
- `gedit`: Opens the default GUI text editor
- `gio`: Opens a file using its default GUI app
- `xdg-open`: Opens a file or URL in the user's preferred application
- `file`: A simple command that determines a file's type
- `mktemp`: Creates temporary files under `/tmp` directly with randomized names
	- Use `-d` to create a directory instead
- `base64`: base64 encode/decode data and print to standard output
---
### System management and monitoring

#### Back to top: [[#Links]]
- `id`: print real and effective user and group IDs
- `sudo sysctl -w fs.inotify.max_user_watches=131070` : increase the limit of file watchers.
- `ncdu` : a tool that shows you the distribution of disk space.
- `free -h` : shows memory statistics in a human readable format.
- `swapon --show` : shows the available swapfiles and their usages.
-  `more /proc/sys/vm/swappiness` : shows the swappiness statistic of the system.
- `chmod`: Allows you to change the a file's permissions
	- This command controls three permissions for the categories in order
	- `rwx` are the three permissions read, write, execute
	- `ugo` are the three categories user, group, others
	- `chmod -R u=rwx,g=,o= <DIRECTORY>`
		- In the command above, `u` means "user" (aka "owner"), `g` means "group," and `o` means "others." The "=" means "set the permissions to the following," and the `rwx` means "read, write and execute." The `g=` and `o=` mean "set group and other permissions to nothing." The `-R` means "recursively," which means "do this to all of the contents of the directory as well."
	- There are multiple valid syntaxes for this command, such as `u+x` or `+x` for adding execute to user, or `-x` for removing execute from user
	- More info can be found at [[#Privileges/permissions]]
- `chown`: Changes the owner of a file or directory
	- `sudo chown -R <user> <DIRECTORY>`

Be sure to replace `DIRECTORY` with the path to the `private` directory.
```zsh
sudo dd if=dev/zero of=/swapfile2 bs=1M count=2048 status=progress
sudo chmod 600 /swapfile2
sudo mkswap /swapfile2
sudo swapon /swapfile2
``` 
A command that starts with dd, a command for reading, writing, and converting file data. The command contains the following parameters:

> if=/dev/zero is the input file. The /dev/zero file is a special file that returns as many null characters as a read operation requests.
of=/swapfile is the output swap storage file. The common practice is to place the file in the root directory.
The bs parameter is the block size.
The count parameter determines how many blocks to copy.

> Followed by chmod 600 to give the swapfile read and write permissions, then mkswap to convert the file size to be reserved for swap, swapon then activates the swapfile (it's best to add the swapfile in /etc/fstab for it to become persistent between restarts)
- Another swap method. It's better on arch, and you should also have a swap folder to avoid swap files being added to btrfs snapshots
```zsh
sudo mkdir /swap
sudo chattr +C /swap
sudo swapoff /swapfile0
sudo truncate -s 0 /swap/swapfile0
sudo chattr +C /swap/swapfile0
sudo fallocate -l 16G /swap/swapfile0
sudo chmod 600 /swap/swapfile0
sudo mkswap /swap/swapfile0
sudo swapon /swap/swapfile0
```
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
	- `sudo mount -t ext4 -o exec,dev,suid UUID=eb360311-93d7-4e5e-8b13-19d4153e6f1b /mnt/ubuntu24`
- `env`: Shows the environment variables on the shell for the current session only
- `set`: Shows the shell's local variables
- `printenv`: Safer method of showing env variables than `echo`
	- Doesn't need `$` before the variable's name
- `sysbench`: Scriptable multi-threaded benchmark tool for databases and systems
- `powertop`:  A power consumption and power management diagnosis tool.
- `snapper`: snapshot creation tool. The created snapshots can be booted into using `limine` or `GRUB`
- `btrfs`: A filesystem and a toolbox for managing said filesystem/
	- `btrfs subvolume list`: shows you all your subvolumes, like those created by snapper
	- `btrfs balance start`: attempts to balance the filesystem blocks when they are fragmented
- `lsblk`: lists block devices, so it can show available SSDs, filesystem types, mountpoints and so on
---
### Networking commands and tools

#### Back to top: [[#Links]]
- `arp`: shows the device's arp table. ^225c89
- `nmap -sn 192.168.1.1/24`: shows all the IPs in the specified range and subnet mask as well as their open ports. Use --verbose on all commands for more details. ^2b1c6e
- `nmcli`: CLI network manager.
- `mtr`: shows both ping and traceroute to a specific IP.
- `nslookup`: This command performs manual DNS queries to convert domain names into IP addresses and also works in reverse.
- `dig`: does DNS lookups and reverse DNS with the -x flag ^d16ff0
	-  It's more detailed that `nslookup`
- `ifconfig`/`ip`: both show IPs and network interface information. ^a631d5
- `netstat`: for network statistics.
- `host`: shows info about a host.
- `hostname`: shows the hostname. Use with -I to see the IPs.
- `iftop`: a network monitoring tool.
- `curl`/`wget`: tools for data transfer/downloading files via the terminal.
	- You can use `curl ifconfig.me` to see your public IP address
	- Also with `wget -qO- ifconfig.me`
	- Some curl commands:
		- `curl https://jsonplaceholder.typicode.com/users/1 > user1.json`
			- curl uses `GET` by default, and in the above example, we redirected the output to a json file
		- `curl -X POST http://example.com/resource -H "Content-Type: application/json" -d '{"key1":"value1","key2":"value2"}'`
			- Here we used `-X` to pick the http method `POST` and `-H` to set the content type header, then `-d` to send the actual json data
			- This post request will return a response that we can redirect to a file again, as the responses are `stdout`
- `xh`: xh uses HTTPie's request-item syntax to set headers, request body, query string, etc.
	-  =/:= for setting the request body's JSON or form fields (= for strings and := for other JSON types).
	- == for adding query strings.
	- @ for including files in multipart requests e.g picture@hello.jpg or picture@hello.jpg;type=image/jpeg;filename=goodbye.jpg.
	- : for adding or removing headers e.g connection:keep-alive or connection:.
	- ; for including headers with empty values e.g header-without-value;.
	- An @ prefix can be used to read a value from a file. For example: x-api-key:@api-key.txt.
	- The request body can also be read from standard input, or from a file using @filename.
- `jq`: A tool for parsing and manipulating JSON data
	- Piping curl request responses to `jq` will immediately parse it in a nice format
	- We can pick a specific field from the response `jq '.name'` or `jq '.name.' user.json` if you use it with a file
	- For arrays we can do `jq '.[].username` for example to get the username field from an array of elements
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
- [[TECH SSH |ssh]]
	- More info on SSH available at [https://help.ubuntu.com/community/SSH/OpenSSH/Keys](https://help.ubuntu.com/community/SSH/OpenSSH/Keysa)
- `scp`: Just check this link for more info [https://help.ubuntu.com/community/SSH/TransferFiles](https://help.ubuntu.com/community/SSH/TransferFiles)
- `tcpdump`: This command captures network packets in real-time, providing insight into traffic flowing through the network.
- `nc (Netcat)`: This command reads and writes data across network connections using TCP or UDP. It is often called the “Swiss army knife” of networking
	- To send data from a file to a specific IP and port
	- `nc <IP> <PORT> < </path/file>`
- `ss`: A modern replacement for netstat, this command analyzes socket-level statistics, such as open and listening ports
	- Syntax: ss -tuln (to show listening TCP/UDP ports)
- `warp-cli`: A cloudflare cli tool for using `warp`
	- initialize with `warp-cli registration new`
	- connect with `warp-cli connect`
	- disconnect with `warp-cli disconnect`
- `openssl`: 
	- `openssl s_client -crlf -connect <ip:port> -servername <ip>`
	- openssl versions older that 1.1.1 required that we send the IP or name of the server twice, once for the actual handshake, and once to specify what we're connecting to, but nowadays, that part is handled automatically, and we only need to pass the `-servername` flag if we're connecting to an IP address and not a FQDN, or, the TLS host needs to be different
	- Use the `-ign_eof` flag to prevent `CONNECTED COMMANDS` from running.
		- More info in `man openssl-s-client`
	- More information available at [https://www.feistyduck.com/library/openssl-cookbook/online/testing-with-openssl/connecting-to-tls-services.html](https://www.feistyduck.com/library/openssl-cookbook/online/testing-with-openssl/connecting-to-tls-services.html)
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
### Photos, audio and video

#### Back to top: [[#Links]]
- `mpv`: command line media player.
- `timg`: command line image and video player.
- `ffmpeg`: media convertor
- `audacity`: multi-track audio editor and recorder
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
	- You can use `su <user>` for the group addition to take effect
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
- `nmap -T4 -p- -A <target-IP-address>`
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