## Recon
nmap
![[Pasted image 20260704134646.png]]

FTP
Nothing can see here on ftp service, only logs
![[Pasted image 20260704213832.png]]

WEB (Port 9998)
![[Pasted image 20260704213905.png]]

Source Code Web
![[Pasted image 20260704214115.png]]
It tells product version 100.0.6919

There's CVE for the SmarterMail for that version (CVE-2019-7214)
![[Pasted image 20260704214305.png]]

Change the RHOST with IP target, LHOST and LPORT with our server
![[Pasted image 20260704214434.png]]

Got Revshell as SYSTEM
![[Pasted image 20260704214553.png]]

