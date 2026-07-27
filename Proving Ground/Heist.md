## Recon
NMAP
![[Pasted image 20260710223140.png]]
There's HTTP service on port 8080

Look's like SSRF things
![[Pasted image 20260710223217.png]]

## Foothold
Valid SSRF
![[Pasted image 20260710223306.png]]

Since this is windows machine, try to get NTLM creds by let the server access to our SMB service which will send NTML creds trough connection

Run responder
```bash
sudo responder -I tun0 -wv
```

![[Pasted image 20260710223458.png]]

let web access to our IP:80
![[Pasted image 20260710223539.png]]

Got NTMLv2 Hash
![[Pasted image 20260710223603.png]]

Crack the Hash using hashcat
![[Pasted image 20260710223658.png]]

Got the password of `enox`, that's `california`

![[Pasted image 20260710223911.png]]

![[Pasted image 20260710224249.png]]
## Privilege Escalation

Get Bloodhound data using bloodhound-ce-python
![[Pasted image 20260711000212.png]]

Upload the zip to bloodhound
![[Pasted image 20260711000238.png]]

Explore what user `enox` can do, it tells there's Outbond to svc_apache user using `ReadGMSAPassword`

![[Pasted image 20260711000001.png]]

Found to read GMSA Password on [github](https://github.com/AlexLinov/Compiled-Binaries)
![[Pasted image 20260712024451.png]]

Got Hash of user `svc_apache$`
![[Pasted image 20260712024523.png]]

Got User `svc_apache$`
![[Pasted image 20260712024556.png]]

## Privilege Escalation v2

There's privilege to `SeRestorePrivilege`
![[Pasted image 20260712024821.png]]

Got compiled on this [repo](https://github.com/AlexLinov/Compiled-Binaries)
![[Pasted image 20260712024858.png]]

Download the payload
![[Pasted image 20260712024927.png]]

Run the payload
![[Pasted image 20260712024944.png]]

Got NT AUTHORITY\SYSTEM
![[Pasted image 20260712025109.png]]


![[Pasted image 20260712024254.png]]

## What I Learned
- Leaking NetNTLM Hashes via SSRF Using UNC Paths ([Medium](https://medium.com/@shubhamsonani/leaking-netntlm-hashes-via-ssrf-using-unc-paths-windows-9c37e17b5041))
- Privilege Escalation using ReadGMSAPassword ([github](https://github.com/AlexLinov/Compiled-Binaries))
- Privilege Escalation using SeRestorePrivilege ([github](https://github.com/AlexLinov/Compiled-Binaries))