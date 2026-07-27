## Recon
![[Pasted image 20260712212648.png]]

Check http://web:80/panel/
![[Pasted image 20260712212731.png]]

Found out using Subrion CMS v4.2.1, and found the CVE
![[Pasted image 20260712212809.png]]

Got user `www-data`
![[Pasted image 20260712212902.png]]

Got suspicious file on /opt
![[Pasted image 20260712212947.png]]

Found that using old exiftool that vuln to RCE
![[Pasted image 20260712213023.png]]

Found CVE
![[Pasted image 20260712213041.png]]

Create Image
![[Pasted image 20260712213114.png]]

Upload the malicious .jpg
![[Pasted image 20260712213153.png]]

Run vuln binary
![[Pasted image 20260712213224.png]]

Got ROOT
![[Pasted image 20260712213238.png]]

![[Pasted image 20260712213250.png]]

## What I Learned
1. Check Service Version
2. Always check /opt