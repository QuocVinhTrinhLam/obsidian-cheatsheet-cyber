## SSH Local Port Forwarding
![[Pasted image 20260516165508.png]]
#### Scanning the Pivot Target

```shell
3kjS@htb[/htb]$ nmap -sT -p22,3306 10.129.202.64 

Starting Nmap 7.92 ( https://nmap.org ) at 2022-02-24 12:12 EST 
Nmap scan report for 10.129.202.64 
Host is up (0.12s latency). 
PORT     STATE    SERVICE 
22/tcp   open     ssh 
3306/tcp closed   mysql 

Nmap done: 1 IP address (1 host up) scanned in 0.68 seconds
```
#### Executing the Local Port Forward

```shell
3kjS@htb[/htb]$ ssh -L 1234:localhost:3306 ubuntu@10.129.202.64
```
#### Confirming Port Forward with Netstat

```shell
3kjS@htb[/htb]$ netstat -antp | grep 1234
```
#### Confirming Port Forward with Nmap

```shell
3kjS@htb[/htb]$ nmap -v -sV -p1234 localhost
```
#### Forwarding Multiple Ports

```shell
3kjS@htb[/htb]$ ssh -L 1234:localhost:3306 -L 8080:localhost:80 ubuntu@10.129.202.64
```
## Setting up to Pivot
#### Looking for Opportunities to Pivot using ifconfig
![[Screenshot 2026-05-18 at 13.52.16.png]]
![[Pasted image 20260518135530.png]]
#### Enabling Dynamic Port Forwarding with SSH

```shell
3kjS@htb[/htb]$ ssh -D 9050 ubuntu@10.129.202.64
```
#### Checking /etc/proxychains.conf

```shell
3kjS@htb[/htb]$ tail -4 /etc/proxychains.conf 

# meanwile 
# defaults set to "tor" 
socks4 127.0.0.1 9050
```
#### Using Nmap with Proxychains

```shell
3kjS@htb[/htb]$ proxychains nmap -v -sn 172.16.5.1-200
```
#### Enumerating the Windows Target through Proxychains

```shell
3kjS@htb[/htb]$ proxychains nmap -v -Pn -sT 172.16.5.19
```
![[Screenshot 2026-05-18 at 14.07.43.png]]
## Using Metasploit with Proxychains

```shell
3kjS@htb[/htb]$ proxychains msfconsole
```
#### Using rdp_scanner Module

```shell
msf6 > search rdp_scanner
```
![[Screenshot 2026-05-18 at 14.08.40.png]]
#### Using xfreerdp with Proxychains

```shell
3kjS@htb[/htb]$ proxychains xfreerdp /v:172.16.5.19 /u:victor /p:pass@123
```
#### Successful RDP Pivot
![[Pasted image 20260518141033.png]]
