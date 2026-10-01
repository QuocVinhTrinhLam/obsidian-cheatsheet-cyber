# [[Miscellaneous File Transfer Methods]]

**Create flag.txt in Attacker by using:**

```Shell
nano flag.txt
pwd
/home/htb-ac-1951749/Desktop
```

**RDP into victim device **

```Shell
┌─[us-academy-5]─[10.10.14.56]─[htb-ac-1951749@htb-sfispzadjl]─[~]
└──╼ [★]$ rdesktop 10.129.201.55 -u htb-student -p HTB_@cademy_stdnt! -r disk:linux='/home/htb-ac-1951749/Desktop'
```
![[Screenshot 2026-05-01 at 11.58.21.png]]

**Access to File Explorer -> Network **
![[Screenshot 2026-05-01 at 11.59.10.png]]

**Then tsclient -> \\\tsclient\linux**![[Screenshot 2026-05-01 at 11.59.50.png]]

