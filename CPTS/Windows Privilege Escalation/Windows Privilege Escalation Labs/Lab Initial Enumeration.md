# [Initial Enumeration](Initial%20Enumeration.md)
### What non-default privilege does the htb-student user have?

```cmd
whoami /priv
```
![](Screenshot%202026-08-14%20at%2015.27.27.png)
### Who is a member of the Backup Operators group?

```cmd
net localgroup "Backup Operators"
```
![](Screenshot%202026-08-14%20at%2015.31.16.png)
### What service is listening on port 8080 (service name not the executable)?

```cmd
netstat -ano | findstr :8080
```
![](Screenshot%202026-08-14%20at%2015.34.18.png)

Then map that PID ( 2108 ) to the service name

```cmd
tasklist /svc /fi "PID eq 2108"
```
![](Screenshot%202026-08-14%20at%2015.35.39.png)
### What user is logged in to the target host?

```cmd
query user
```
![](Screenshot%202026-08-14%20at%2015.37.24.png)
### What type of session does this user have?

Look at the query output above

```
USERNAME       SESSIONNAME    ID   STATE    IDLE TIME   LOGON TIME
sccm_svc       console        1    Active   none        8/14/2026 12:55 AM
htb-student    rdp-tcp#0      2    Active   .           8/14/2026 12:58 AM
```
