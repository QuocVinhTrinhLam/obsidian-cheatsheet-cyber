#### Installing sshuttle

```shell
3kjS@htb[/htb]$ sudo apt-get install sshuttle
```
#### Running sshuttle

```shell
3kjS@htb[/htb]$ sudo sshuttle -r ubuntu@10.129.202.64 172.16.5.0/23 -v
```
#### Traffic Routing through iptables Routes

```shell
3kjS@htb[/htb]$ sudo nmap -v -A -sT -p3389 172.16.5.19 -Pn
```
![[Screenshot 2026-05-19 at 13.00.45.png]]
