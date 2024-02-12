# CVE-2024-25317 - Hotel Manangment System - Cross Site Scripting
+ *Exploit Title:* Hotel Manangment System - Cross Site Scripting
+ *Date:* 2024-20-01
+ *Exploit Author:* Tuba Kavgacı
+ *Vendor Homepage:* https://www.sourcecodester.com/php/17115/travel-journal-using-php-and-mysql-source-code.html
+ *Software Link:* https://www.sourcecodester.com/download-code?nid=17115&title=Travel+Journal+Using+PHP+and+MySQL+with+Source+Code
+ *Version:* 1.0
+ *Tested on:* Kali Linux + PHP 8.2.12, Apache 2.4.58
+ *CVE:* CVE-2024-25317


## Description:
Hotel Managment System 1.0 allows mirrored cross-site scripting with the 'Firstname' parameter in the `http://localhost/Hotel/admin/reservation.php` path. Hotel Managment System is vulnerable to a cross-site scripting vulnerability because it fails to adequately sanitize user-supplied data.
An attacker could exploit this issue to run arbitrary scripting code in the browser of an unsuspecting user in the context of the affected site. This could allow an attacker to steal cookie-based authentication credentials and launch other attacks.


## Proof of Concept:
+ Go to http://localhost/Hotel/admin/reservation.php
+ Write this code in the Firstname : `"><img src=x onerror=alert(document.domain)>`
+ Then click 'Submit' button.
+ Go to http://localhost/Hotel/admin/home.php
+ XSS will be triggered

![Ekran görüntüsü 2024-02-03 041150](https://github.com/tubakvgc/CVEs/assets/74067343/fc28e9f4-08e8-4e51-9d51-7eacbed80495)

![Ekran görüntüsü 2024-02-03 041218](https://github.com/tubakvgc/CVEs/assets/74067343/6a020c1f-4dbd-40a6-b620-d8b991e394bc)

