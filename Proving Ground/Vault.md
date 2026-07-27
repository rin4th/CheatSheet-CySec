## Recon
![[Pasted image 20260717232440.png]]

There's SMB Service

### Enumerate SMB
![[Pasted image 20260718000608.png]]

## Foothold
On SMB we have Write permission on share DocumentShare. We can try to upload malicious .lnk

Create malicious lnk
![[Pasted image 20260718000945.png]]

Turn On Responder
![[Pasted image 20260718001057.png]]

Upload .lnk file
![[Pasted image 20260718002043.png]]

Got Response from responder
![[Pasted image 20260718002758.png]]

Crack the hash
![[Pasted image 20260718002922.png]]

## Intiall Access
![[Pasted image 20260718003206.png]]

## Understanding Active Directory
### Dump Bloodhound
![[Pasted image 20260718003253.png]]

![[Pasted image 20260718003504.png]]

Abusing GPO
![[Pasted image 20260718003646.png]]

![[Pasted image 20260718003725.png]]





![[Pasted image 20260717230401.png]]

## What I Learned
- try username "guest" without password on SMB
- If `guest` user on SMB got permission WRITE on Directory, try to put malicious lnk