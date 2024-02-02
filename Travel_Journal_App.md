# CVE-2024-24041 - Travel Journal App - Cross Site Scripting
+ *Exploit Title:* Travel Journal App - Cross Site Scripting
+ *Date:* 2024-20-01
+ *Exploit Author:* Tuba Kavgacı
+ *Vendor Homepage:* https://www.sourcecodester.com/php/17115/travel-journal-using-php-and-mysql-source-code.html
+ *Software Link:* https://www.sourcecodester.com/download-code?nid=17115&title=Travel+Journal+Using+PHP+and+MySQL+with+Source+Code
+ *Version:* 1.0
+ *Tested on:* Windows 11 Home + PHP 8.2.12, Apache 2.4.58
+ *CVE:* CVE-2024-24041

## References: 
+ https://portswigger.net/web-security/cross-site-scripting

## Description:
Travel Journal App 1.0 allows mirrored cross-site scripting of the 'Location' and 'Share your monents' form's in the path `http://localhost/travel-journal/write-journal.php`. Travel Journal App is vulnerable to a cross-site scripting vulnerability because it fails to adequately sanitise user-supplied data.
An attacker could exploit this issue to run arbitrary script code in the browser of an unsuspecting user in the context of the affected site. This could allow an attacker to steal cookie-based authentication credentials and launch other attacks.

## Proof of Concept:
+ Go to 'Write Journal'.
+ Go to the 'Location' or 'Share your monents' forms and use the following payloads: `<script>alert(document.cookie)</script>` and `<script>alert(document.domain)</script>`
+ Then click 'Save Journal'.
+ XSS will be loaded.
+ Go to the 'Read Journal' section and click. 
+ Warnings will pop up.
# Exploting:
  *1:*
  
  ![Ekran görüntüsü 2024-01-20 010845](https://github.com/tubakvgc/CVE3/assets/74067343/55250927-561b-47e7-8851-66a3daa0b8b2)

  *2:*

  ![Ekran görüntüsü 2024-01-20 010924](https://github.com/tubakvgc/CVE3/assets/74067343/5e111184-addc-4772-9029-1a13a8558695)

  *3:*

  ![Ekran görüntüsü 2024-01-20 011126](https://github.com/tubakvgc/CVE3/assets/74067343/9445bb44-c202-446c-a614-a410b453bc54)


  *4:*

  ![Ekran görüntüsü 2024-01-20 011215](https://github.com/tubakvgc/CVE3/assets/74067343/b7907353-d119-4f87-b4da-8546cc368b5f)

  *5:*
  
  ![Ekran görüntüsü 2024-01-20 011351](https://github.com/tubakvgc/CVE3/assets/74067343/bbc47f03-fdc7-4a20-80b9-ddbb03c1ecca)


  *6:*
  
  ![Ekran görüntüsü 2024-01-20 011453](https://github.com/tubakvgc/CVE3/assets/74067343/f394a8ae-ed0f-47fb-9f8c-3bc8cb89fe4c)


