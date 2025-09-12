---
tags: 
- type/reference/linux
- topic/cli
---
# Links: 
- [[#Various command line tools]]
- [[#File operations]]
- [[#Web development project init]]
- [[#System management and monitoring]]
- [[#Networking commands and tools]]
- [[#System protection]]
- [[#System navigation]]
- [[#Photos and video]]
- [[#Ghostty]]
- [[#Gaming]]
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
- `wc`: word count
- `uniq`: finds unique occurrences of lines in the file 
- `cut`: extracts specific columns from a file
- `diff`: compares the contents of files
- `sort`: sorts the contents of the file
- #### Back to top: [[#Links]]
---
### Web development project init

#### Back to top: [[#Links]]
- `pnpm create next-app` : creats a next.js app
- `pnpm i -h @nestjs/cli nest new "appname"`
- `pnpm exec tsc -b`: Runs typescript's compiler to check for errors
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

> Followed by chmod 600 to give the swapfile read and write permissions, then mkswap to convert the file size to be reserved for swap, swapon then activates the swapfile (it's best to add the swapfile in /etc/fstab for it to become persistant between restarts)

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
		1. `python3 -m http.server [port number]`
		2. The webserver starts within the folder you're currently inside
---
### Networking commands and tools

#### Back to top: [[#Links]]
- `arp`: shows the device's arp table. ^225c89
- `nmap -sn 192.168.1.1/24`: shows all the IPs in the specified range and subnet mask as well as their open ports. Use --verbose on all commands for more details. ^2b1c6e
	- See also [[Useful Linux Commands For Pentesting#nmap|nmap]]
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
```
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

#### Back to top: [[#Links]]
- `Ranger` : file management tool.
- `rofi` : window switcher that can also run commands and browse files.
---
### Photos and video

#### Back to top: [[#Links]]
- `mpv`: command line media player.
- `timg`: command line image and video player.
---
### Ghostty

#### Back to top: [[#Links]]
- `ghostty +show-config --default --docs`: prints ghostty docs to stdout (use with vipe to get it in nvim)
---
### Gaming

#### Back to top: [[#Links]]
- How to run a game with the discrete GPU :
`__NV_PRIME_RENDER_OFFLOAD=1 __GLX_VENDOR_LIBRARY_NAME=nvidia __VK_LAYER_NV_optimus=NVIDIA_only wine /mnt/32E0DD386AB19F0B/Games/Exanima.v0.9.0.5/Exanima.v0.9.0.5/Exanima.exe`
---