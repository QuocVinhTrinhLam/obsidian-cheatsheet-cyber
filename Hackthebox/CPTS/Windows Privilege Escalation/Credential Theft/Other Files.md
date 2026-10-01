## Manually Searching the File System for Credentials

We can search the file system or share drive(s) manually using the following commands from [this cheatsheet](https://swisskyrepo.github.io/InternalAllTheThings/redteam/escalation/windows-privilege-escalation/).
#### Search File Contents for String - Example 1

```cmd
C:\htb> cd c:\Users\htb-student\Documents & findstr /SI /M "password" *.xml *.ini *.txt 

stuff.txt
```
#### Search File Contents for String - Example 2

```cmd
C:\htb> findstr /si password *.xml *.ini *.txt *.config 

stuff.txt:password: l#-x9r11_2_GL!
```
#### Search File Contents for String - Example 3

```cmd
C:\htb> findstr /spin "password" *.* 

stuff.txt:1:password: l#-x9r11_2_GL!
```
#### Search File Contents with PowerShell

```cmd
PS C:\htb> select-string -Path C:\Users\htb-student\Documents\*.txt -Pattern password 

stuff.txt:1:password: l#-x9r11_2_GL!
```
#### Search for File Extensions - Example 1

```cmd
C:\htb> dir /S /B *pass*.txt == *pass*.xml == *pass*.ini == *cred* == *vnc* == *.config* 

c:\inetpub\wwwroot\web.config
```
#### Search for File Extensions - Example 2

```cmd
C:\htb> where /R C:\ *.config 

c:\inetpub\wwwroot\web.config
```
#### Search for File Extensions Using PowerShell

```powershell
PS C:\htb> Get-ChildItem C:\ -Recurse -Include *.rdp, *.config, *.vnc, *.cred -ErrorAction Ignore
```
## Sticky Notes Passwords

This file is located at `C:\Users\<user>\AppData\Local\Packages\Microsoft.MicrosoftStickyNotes_8wekyb3d8bbwe\LocalState\plum.sqlite` and is always worth searching for and examining.

```powershell
PS C:\htb> ls
 
 
    Directory: C:\Users\htb-student\AppData\Local\Packages\Microsoft.MicrosoftStickyNotes_8wekyb3d8bbwe\LocalState
 
 
Mode                 LastWriteTime         Length Name
----                 -------------         ------ ----
-a----         5/25/2021  11:59 AM          20480 15cbbc93e90a4d56bf8d9a29305b8981.storage.session
-a----         5/25/2021  11:59 AM            982 Ecs.dat
-a----         5/25/2021  11:59 AM           4096 plum.sqlite
-a----         5/25/2021  11:59 AM          32768 plum.sqlite-shm
-a----         5/25/2021  12:00 PM         197792 plum.sqlite-wal
```
![](Other%20Files-20260824-090503.png)
#### Viewing Sticky Notes Data Using PowerShell

```powershell
PS C:\htb> Set-ExecutionPolicy Bypass -Scope Process
```
![](Screenshot%202026-08-24%20at%2009.05.47.png)

```powershell
PS C:\htb> cd .\PSSQLite\
PS C:\htb> Import-Module .\PSSQLite.psd1
PS C:\htb> $db = 'C:\Users\htb-student\AppData\Local\Packages\Microsoft.MicrosoftStickyNotes_8wekyb3d8bbwe\LocalState\plum.sqlite'
PS C:\htb> Invoke-SqliteQuery -Database $db -Query "SELECT Text FROM Note" | ft -wrap
```
![](Screenshot%202026-08-24%20at%2009.06.02.png)
#### Strings to View DB File Contents

```shell
3kjS@htb[/htb]$ strings plum.sqlite-wal
```
## Other Files of Interest
#### Other Interesting Files

```shell
%SYSTEMDRIVE%\pagefile.sys
%WINDIR%\debug\NetSetup.log
%WINDIR%\repair\sam
%WINDIR%\repair\system
%WINDIR%\repair\software, %WINDIR%\repair\security
%WINDIR%\iis6.log
%WINDIR%\system32\config\AppEvent.Evt
%WINDIR%\system32\config\SecEvent.Evt
%WINDIR%\system32\config\default.sav
%WINDIR%\system32\config\security.sav
%WINDIR%\system32\config\software.sav
%WINDIR%\system32\config\system.sav
%WINDIR%\system32\CCM\logs\*.log
%USERPROFILE%\ntuser.dat
%USERPROFILE%\LocalS~1\Tempor~1\Content.IE5\index.dat
%WINDIR%\System32\drivers\etc\hosts
C:\ProgramData\Configs\*
C:\Program Files\Windows PowerShell\*
```
