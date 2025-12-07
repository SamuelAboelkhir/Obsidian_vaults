---
tags:
- Academy
- Assessment
- Flow
- EH
MOC: IT
---
[[_0000 Home|Home]] | [[_0005 IT MOC|Back to IT MOC]] | [[EH AS Academy index|Back to index]]
# Complete flow to gain root
1. nmap to find the available ports
2. FTP to find the note.txt file with leaked credentials
3. Hash cracker tool to figure out the hashed password
4. Dirbuster/Dirb/ffuf to find all the available URLs on port 80
5. From the academy URL enter the credentials
6. Go to the upload file feature, and upload the PHP reverse shell script
	1. Make sure you have a netcat listener on the designated port
7. After gaining access, use getent passwd to find who the users are
8. Set up a quick python server and upload linpeas.sh and pspy
9. wget linpeas.sh to the machine, make it executable and run it
10. Find grimmie's password in linpeas and ssh to the machine with his credentials
11. Use pspy to confirm that grimmie's backup.sh runs chronologically
12. Add a 1 liner bash reverse shell code to it, assuming that root runs this script
13. Gain root access