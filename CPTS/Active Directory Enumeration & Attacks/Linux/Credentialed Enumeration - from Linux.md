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
## Windapsearch
#### Windapsearch - Domain Admins

```shell
3kjS@htb[/htb]$ python3 windapsearch.py --dc-ip 172.16.5.5 -u forend@inlanefreight.local -p Klmcargo2 --da
```
#### Windapsearch - Privileged Users

```shell
3kjS@htb[/htb]$ python3 windapsearch.py --dc-ip 172.16.5.5 -u forend@inlanefreight.local -p Klmcargo2 -PU
```
## Bloodhound.py
#### BloodHound.py Options

```shell
3kjS@htb[/htb]$ bloodhound-python -h
```
#### Executing BloodHound.py

```shell
3kjS@htb[/htb]$ sudo bloodhound-python -u 'forend' -p 'Klmcargo2' -ns 172.16.5.5 -d inlanefreight.local -c all
```
#### Viewing the Results

```shell
3kjS@htb[/htb]$ ls 

20220307163102_computers.json   20220307163102_domains.json 

20220307163102_groups.json   20220307163102_users.json
```
#### Upload the Zip File into the BloodHound GUI

We could then type `sudo neo4j start` to start the [neo4j](https://neo4j.com/) service, firing up the database we'll load the data into and also run Cypher queries against.

Next, we can type `bloodhound` from our Linux attack host when logged in using `freerdp` to start the BloodHound GUI application and upload the data. The credentials are pre-populated on the Linux attack host, but if for some reason a credential prompt is shown, use:

- `user == neo4j` / `pass == HTB_@cademy_stdnt!`.
#### Uploading the Zip File
![[Pasted image 20260525170841.png]]
#### Searching for Relationships
![[Pasted image 20260525170921.png]]
