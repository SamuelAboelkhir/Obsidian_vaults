More tools are available at [[Useful Command-line Tools#System management and monitoring]]

### Finding Users Data
- `Getent` with group/passwd/shadow(with sudo).  
-  Group  will show you all members of a group
-  Passwd will show you all users
-  Shadow will show you the password hashes of users that actually have passwords

### Privileges
-  Privileges on Linux follow the structure drwxrwxrwx
-  This line is split into d (directory) 1-rwx 2-rwx 3-rwx where each set represents a group: user/owner, group, and other users respectively
- This line shown by ls -la is normally followed by two columns | user group | showing the user name and the group name that have access to the file with rwx defining their privileges being read, write, and execute
- You can change a file's privileges with `chmod` using +rwx or -rwx to add or remove privileges
- Chmod also works with the following totals
![Chmod privileges](assets/Screenshot%20from%202025-08-24%2003-55-14.png)