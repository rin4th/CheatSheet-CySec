![[Pasted image 20260815212947.png]]

Found LFI
![[Pasted image 20260815213014.png]]

Found two User
![[Pasted image 20260815213042.png]]

success LFI
![[Pasted image 20260815213131.png]]

Found id_rsa of Viewer user
![[Pasted image 20260815213224.png]]

Successfully got user
![[Pasted image 20260815215245.png]]


Summary
1. Intial Access : Login to web as admin:admin (default creds)
2. Found there's two user
3. found the web is vuln to LFI
4. read C:\Users\Viewer\\.ssh\id_rsa
5. login ssh as viewer
6. on the same service there's vuln "Weak Password Encryption"
7. run the exploit and adjust the password with  encrypted password of administrator
8. last character missing, bruteforce every character with runas
9. found the creds `14WatchD0g$`
10. runas nc.exe as administrator to penelope
11. got System