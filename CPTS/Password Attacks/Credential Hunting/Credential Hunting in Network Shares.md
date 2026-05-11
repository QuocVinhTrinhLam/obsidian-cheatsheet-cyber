## Hunting from Windows
#### Snaffler

```cmd
c:\Users\Public>Snaffler.exe -s
```
#### PowerHuntShares
[PowerHuntShares](https://github.com/NetSPI/PowerHuntShares)
![[Pasted image 20260510202141.png]]
We can run a basic scan using `PowerHuntShares` like so:

```cmd
PS C:\Users\Public\PowerHuntShares> Invoke-HuntSMBShares -Threads 100 -OutputDirectory c:\Users\Public
```
## Hunting from Linux
#### MANSPIDER

```shell
3kjS@htb[/htb]$ docker run --rm -v ./manspider:/root/.manspider blacklanternsecurity/manspider 10.129.234.121 -c 'passw' -u 'mendres' -p 'Inlanefreight2025!'
```
#### NetExec

```shell
3kjS@htb[/htb]$ nxc smb 10.129.234.121 -u mendres -p 'Inlanefreight2025!' --spider IT --content --pattern "passw"
```
