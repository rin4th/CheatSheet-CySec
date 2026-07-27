## Recon
### NMAP
![[Pasted image 20260705174532.png]]

check CVE port 443
![[Pasted image 20260707210500.png]]

Didn't work
![[Pasted image 20260707210528.png]]

## Foothold
Found upload function
![[Pasted image 20260707212138.png]]

#### Upload .php%00.png
Failed

#### Upload .htaccess
![[Pasted image 20260707224858.png]]

![[Pasted image 20260707225018.png]]

![[Pasted image 20260707225007.png]]

![[Pasted image 20260707225110.png]]

#### Sharphound
![[Pasted image 20260707225156.png]]

###  kerberos
Found user with kerberosable
![[Pasted image 20260707224506.png]]

#### Rubeus
![[Pasted image 20260707225232.png]]

Cracked
![[Pasted image 20260707225331.png]]

Sincer there's no service such as winrm or rdp, quite confused to use the creds. even on smb there's nothing interesting

![[Pasted image 20260707225533.png]]

### Runas
Since there's runas but can't input the password, using [Invoke-Runas](https://github.com/antonioCoco/RunasCs)
![[Pasted image 20260707225911.png]]

Create the payloads.exe to revshell
![[Pasted image 20260707225957.png]]

Got svc_mssql
![[Pasted image 20260707230031.png]]

There's SeManageVolumePrivilege
![[Pasted image 20260707230127.png]]

![[Pasted image 20260707230212.png]]
![[Pasted image 20260707230232.png]]

![[Pasted image 20260707230453.png]]

![[Pasted image 20260707230513.png]]

![[Pasted image 20260707230531.png]]


![[Pasted image 20260707224617.png]]
## What learn
1. SeManageVolume to Privilege Escalation
2. if found creds and can't winrm or rdp, try to runas/Invoke-Runas
3. if get data bloodhound, find Kerberostable and asreproastable