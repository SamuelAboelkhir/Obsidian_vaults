---
Title: Useful Command-line Tools
tags: [Personal, General]
---

### Various command line tools

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
### Web development project init

- `pnpm create next-app` : creats a next.js app
- `pnpm i -h @nestjs/cli nest new "appname"`
- `pnpm exec tsc -b`: Runs typescript's compiler to check for errors
---
### System management and monitoring

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
---
### Networking commands and tools

- `arp`: shows the device's arp table.
- `nmap -sn 192.168.1.1/24`: shows all the IPs in the specified range and subnet mask as well as their open ports. Use --verbose on all commands for more details.
- `nmcli`: CLI network manager.
- `mtr`: shows both ping and traceroute to a specific IP.
- `dig`: does DNS lookups and reverse DNS with the -x flag.
- `ifconfig`/`ip`: both show IPs and network interface information.
- `netstat`: for network statistics.
- `host`: shows info about a host.
- `hostname`: shows the hostname. Use with -I to see the IPs.
- `iftop`: a network monitoring tool.
- `curl`/`wget`: tools for data transfer/downloading files via the terminal.
- `Lynx` : a simple terminal web browser.
- `lsof -i [port]`: Shows the process that owns or is using the port.
- `termshark`: CLI tshark.
---
### System protection

- `ufw`: local firewall.
- `clamAV`: anti-virus.
---
### System navigation

- `Ranger` : file management tool.
- `rofi` : window switcher that can also run commands and browse files.
---
### Photos and video

- `mpv`: command line media player.
- `timg`: command line image and video player.
---
### Ghostty
- `ghostty +show-config --default --docs`: prints ghostty docs to stdout (use with vipe to get it in nvim)
---
### Gaming

- How to run a game with the discrete GPU :
`__NV_PRIME_RENDER_OFFLOAD=1 __GLX_VENDOR_LIBRARY_NAME=nvidia __VK_LAYER_NV_optimus=NVIDIA_only wine /mnt/32E0DD386AB19F0B/Games/Exanima.v0.9.0.5/Exanima.v0.9.0.5/Exanima.exe\`
---