More tools are available at [[Useful Command-line Tools]]

### Finding Users Data
- `Getent` with group/passwd/shadow(with sudo).  
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
- `ifconfig`
- `arp -a`
- `route`

- `ping` shows you if a machine is on the network via an ICMP request. Note that a host machine may have ICMP disabled and appear to not be connected