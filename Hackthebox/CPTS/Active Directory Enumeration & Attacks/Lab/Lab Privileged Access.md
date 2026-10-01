# [Privileged Access](Privileged%20Access.md)

### What other user in the domain has CanPSRemote rights to a host?

```raw data
MATCH p1=shortestPath((u1:User)-[r1:MemberOf*1..]->(g1:Group)) MATCH p2=(u1)-[:CanPSRemote*1..]->(c:Computer) RETURN p2
```
![](Screenshot%202026-05-30%20at%2014.40.01.png)
### What host can this user access via WinRM? (just the computer name)
![](Screenshot%202026-05-30%20at%2014.41.11.png)
### Leverage SQLAdmin rights to authenticate to the ACADEMY-EA-DB01 host (172.16.5.150). Submit the contents of the flag at C:\Users\damundsen\Desktop\flag.txt.

```shell
mssqlclient.py INLANEFREIGHT/DAMUNDSEN@172.16.5.150 -windows-auth
```
![](Pasted%20image%2020260530151120.png)
