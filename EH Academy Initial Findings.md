---
tags:
- Academy
- Assessment
- Findings/Initial
- EH
MOC: EH
---
[[_0000 Home|Home]] | [[_0007 Ethical Hacking MOC|Back to Ethical Hacking MOC]] | [[EH Academy index|Back to index]]
- port21 ftp version
	- [+] 192.168.57.9:21       - FTP Banner: '220 (vsFTPd 3.0.3)\x0d\x0a'
- port80
	- Apache/2.4.38 (Debian) Server
# Found note via FTP
Hello Heath !
Grimmie has setup the test website for the new academy.
I told him not to use the same password everywhere, he will change it ASAP.


I couldn't create a user via the admin panel, so instead I inserted directly into the database with the following command:

INSERT INTO `students` (`StudentRegno`, `studentPhoto`, `password`, `studentName`, `pincode`, `session`, `department`, `semester`, `cgpa`, `creationdate`, `updationDate`) VALUES
('10201321', '', 'cd73502828457d15655bbd7a63fb0bc8', 'Rum Ham', '777777', '', '', '', '7.60', '2021-05-29 14:36:56', '');

The StudentRegno number is what you use for login.


Le me know what you think of this open-source project, it's from 2020 so it should be secure... right ?
We can always adapt it to our needs.

-jdelta

# Found website
- 192.168.57.9/academy
- http://192.168.57.9/phpmyadmin/index.php
# Website findings
- The upload photo feature allows any file type to be uploaded
	- A PHP reverse shell script can be uploaded to gain root