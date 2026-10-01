# [Communication with Processes](Communication%20with%20Processes.md)
### What service is listening on 0.0.0.0:21? (two words)

```cmd
netstat -ano

tasklist /svc | findstr 2068
```
![](Screenshot%202026-08-18%20at%2016.28.04.png)
### Which account has WRITE_DAC privileges over the \pipe\SQLLocal\SQLEXPRESS01 named pipe?

```cmd
.\accesschk.exe \pipe\SQLLocal\SQLEXPRESS01 -v
```
![](Screenshot%202026-08-18%20at%2016.41.46.png)