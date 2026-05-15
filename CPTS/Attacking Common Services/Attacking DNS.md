## Enumeration

```shell
3kjS@htb[/htb]$ nmap -p53 -Pn -sV -sC 10.10.110.21
```
## DNS Zone Transfer
#### DIG - AXFR Zone Transfer

```shell
3kjS@htb[/htb]$ dig AXFR @ns1.inlanefreight.htb inlanefreight.htb
```

#### Fierce

```shell
3kjS@htb[/htb]$ fierce --domain zonetransfer.me
```
## Domain Takeovers & Subdomain Enumeration
#### Subdomain Enumeration

```shell
3kjS@htb[/htb]$ ./subfinder -d inlanefreight.com -v
```
#### Subbrute

```shell
3kjS@htb[/htb]$ git clone https://github.com/TheRook/subbrute.git >> /dev/null 2>&1 
3kjS@htb[/htb]$ cd subbrute 
3kjS@htb[/htb]$ echo "ns1.inlanefreight.com" > ./resolvers.txt 
3kjS@htb[/htb]$ ./subbrute.py inlanefreight.com -s ./names.txt -r ./resolvers.txt
```

The tool has found four subdomains associated with `inlanefreight.com`. Using the `nslookup` or `host` command, we can enumerate the `CNAME` records for those subdomains.

```shell
3kjS@htb[/htb]$ host support.inlanefreight.com
```
## DNS Spoofing
#### Local DNS Cache Poisoning
DNS Cache Poisoning using MITM tools like [Ettercap](https://www.ettercap-project.org/) or [Bettercap](https://www.bettercap.org/).

```shell
3kjS@htb[/htb]$ cat /etc/ettercap/etter.dns 

inlanefreight.com A 192.168.225.110 
*.inlanefreight.com A 192.168.225.110
```
![[Pasted image 20260515121849.png]]
Activate `dns_spoof` attack by navigating to `Plugins > Manage Plugins`. This sends the target machine with fake DNS responses that will resolve `inlanefreight.com` to IP address `192.168.225.110`:
![[Pasted image 20260515121928.png]]