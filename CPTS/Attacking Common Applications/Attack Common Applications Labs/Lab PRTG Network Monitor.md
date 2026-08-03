# [PRTG Network Monitor](PRTG%20Network%20Monitor.md)
### What version of PRTG is running on the target?
![](Screenshot%202026-07-30%20at%2014.20.04.png)
### Attack the PRTG target and gain remote code execution. Submit the contents of the flag.txt file on the administrator Desktop.
![](Screenshot%202026-07-30%20at%2014.23.32.png)
![](Screenshot%202026-07-30%20at%2014.42.39.png)

```shell
sudo crackmapexec smb 10.129.80.128 -u prtgadm1 -p Pwn3d_by_PRTG!
```
![](Screenshot%202026-07-30%20at%2014.43.23.png)

```shell
smbclient -L \\10.129.80.128 -U prtgadm1
```
![](Screenshot%202026-07-30%20at%2014.44.26.png)
![](Screenshot%202026-07-30%20at%2014.46.54.png)
![](Screenshot%202026-07-30%20at%2014.47.16.png)
