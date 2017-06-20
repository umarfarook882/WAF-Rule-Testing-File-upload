# WAF Rule Testing (Unrestricted File Upload Vulnerability)
Testing Unrestricted File upload vulnerability on xvwa application with OWASP CRS && CWAF 1.128 (latest version) Ruleset. 


**Testing Unrestricted File Upload Vulnerability on  OWASP CRS:**

```

OWASP CRS block possible malicious files upload i.e .php files from getting compromised by .php shell
but still we can find little flaw in the OWASP CRS

REQUEST-933-APPLICATION-ATTACK-PHP 
rule ID:933110 
PHP support Extension: .php, .phtml, .php3, .php4, .php5, .php7, .phps 
Regex Pattern: .*\.(?:php\d*|phtml)\.*$

.  - Any character
*  - Zero or more character
\d - A digit [0-9]
?: - A non-capturing group 

Test the regex on http://www.regextester.com/ or other  online regex testing  website.
```
*Try to understand how actually the regex detect the file extension .php?*


**Regex Overview:** <br>
```
Regex pattern detect, if the user upload a file as  .php or .php1 or .php2 or .php3 or .php4 or .php5 or .php7, but it failed to detect the .phps extension, because they never included the .phps  in regex pattern to detected by OWASP CRS.

Anyway it not a bug :), even the file .phps was uploaded successfully in the server, it has no use, as default cofiguration on apache  php5 or php7.conf in /etc/apache2/mods-available/ will denied the file with .phps to get executed on apache server. 

phase 1:We have successfully upload a .phps file on the server
```
<br>

**phase 2: Lets think little different scenario, if .htaccess has been enabled on this apache server. then it going to be a 
loop hole for us to get easily bypass OWASP CRS**

As far i anlayzed  OWASP CRS, it does not have any rules to block uploading `.htacess` file :).

So we need to create .htaccess  file to allow .phps to execute as file type **application/x-httpd-php** (or)
we can allow any file extension which is not be detected by the OWASP CRS can be execute as application/x-httpd-php. `i.e  .phps as .php  (or)  jpg as .php`

Let created a .htaccess file :) <br>

```
#.htaccess file to be uploaded on the vulnerable server
<FilesMatch ".+\.phps$">
    SetHandler application/x-httpd-php
    Require all granted
</FilesMatch>
```
 
once you have upload the .htacess file in the server, then let check the  previously upoaded .phps file  in the browser.<br>
.phps file will be  sucessfully executed on server and we get phpinfo page. 

``Finally we have bypassed OWASP CRS in uploading the .php shell``

----------------------------------------------------------------------------------------------------------------------------

**Testing Unrestricted File Upload Vulnerability on CWAF 1.128**

```
When i am started testing the CWAF, it does not take much time to bypass file upload restriction. Becuase when i am going through the CWAF rules, i understand they have  no rules to block malicious file upload `i.e .php file`, which make us easily to upload any .php file on the server. :) 

CWAF failed to detect these following payload, which lead us to upload shell on the server.
.php, .phtml, .php3, .php4, .php5, .php7, .phps

Let try uploading the C99 or 404 error shell. :)
We have upload 404 shell on the server and got acess to the internal server path
```
``Finally we are able to bypass OWASP && CWAF ruleset for uploading .php shell in the server.``


**Final Overview:**<br>
**Unrestricted File Upoad : A5 Security Misconfiguration OWASP TOP 10]**

OWASP CRS: ``Good || Hard to bypass, but still it possible depend up on the scenario like .htacess enabled on the server && no rule to block .htaccess file upload.``

CWAF 1.128 : `` Bad || No rules to block malicious file upload, more rules should be updated  to prevent application from  common OWASP Top 10 vulnerability || rules should be deployed & tested in real time environment  i.e testing rules on vulnerable application
``

**Note:**
``OWASP CRS does not block uploading other flle extension like .exe, .py , .sh ..etc, it block only  .php file upload. becuase based on the application, .php shell upload has high impact on the server and application level. so regex was written in OWASP CRS to block only .php file upload``

## Demo Video
  
   [![Alt text](https://img.youtube.com/vi/lWoxAjvgiHs/0.jpg)](https://www.youtube.com/watch?v=lWoxAjvgiHs)

## Support !
 Email address: umarfarookmech712@gmail.com  for more details. <br>
 Youtube: [FOS](https://www.youtube.com/channel/UCEBHO0kD1WFvIhf9wBCU-VQ) <br>
 Blog: [FOS](https://fosecurity.blogspot.com) 

## Useful links:
 1. [Modsecurity](www.modsecurity.com/)
 2. [Kali](https://www.kali.org/)
 3. [Debuggex](https://www.debuggex.com/)
 4. [XVWA](https://github.com/s4n7h0/xvwa)
 5. [Modsecurity Reference Manual](https://github.com/SpiderLabs/ModSecurity/wiki/Reference-Manual#UNIX)

  
