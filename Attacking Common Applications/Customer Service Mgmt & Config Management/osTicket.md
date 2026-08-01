## Footprinting/Discovery/Enumeration
![](Pasted%20image%2020260801200324.png)

```url
http://support.inlanefreight.local/
```
![](Pasted%20image%2020260801200338.png)
## Attacking osTicket

```url
http://support.inlanefreight.local/open.php
```
![](Pasted%20image%2020260801200927.png)

```url
http://support.inlanefreight.local/open.php
```
![](Pasted%20image%2020260801200938.png)

```url
http://support.inlanefreight.local/open.php
```
![](Pasted%20image%2020260801201002.png)
## osTicket - Sensitive Data Exposure

During our OSINT and information gathering, we discover several user credentials using the tool [Dehashed](http://dehashed.com/) (for our purposes, the sample data below is fictional).

```shell
3kjS@htb[/htb]$ sudo python3 dehashed.py -q inlanefreight.local -p
```

```url
http://support.inlanefreight.local/scp/login.php
```
![](Pasted%20image%2020260801201042.png)

```url
http://support.inlanefreight.local/scp/login.php
```
![](Pasted%20image%2020260801201050.png)

```url
http://support.inlanefreight.local/scp/login.php
```
![](Pasted%20image%2020260801201059.png)
