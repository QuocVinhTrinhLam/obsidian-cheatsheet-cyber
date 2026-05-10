## Search tools
#### Windows Search
![[Pasted image 20260510144435.png]]
#### LaZagne
|Module|Description|
|---|---|
|browsers|Extracts passwords from various browsers including Chromium, Firefox, Microsoft Edge, and Opera|
|chats|Extracts passwords from various chat applications including Skype|
|mails|Searches through mailboxes for passwords including Outlook and Thunderbird|
|memory|Dumps passwords from memory, targeting KeePass and LSASS|
|sysadmin|Extracts passwords from the configuration files of various sysadmin tools like OpenVPN and WinSCP|
|windows|Extracts Windows-specific credentials targeting LSA secrets, Credential Manager, and more|
|wifi|Dumps WiFi credentials|

```cmd
C:\Users\bob\Desktop> start LaZagne.exe all
```
#### findstr

```shell
C:\> findstr /SIM /C:"password" *.txt *.ini *.cfg *.config *.xml *.git *.ps1 *.yml
```
## Additional considerations

- Passwords in Group Policy in the SYSVOL share
- Passwords in scripts in the SYSVOL share
- Password in scripts on IT shares
- Passwords in `web.config` files on dev machines and IT shares
- Password in `unattend.xml`
- Passwords in the AD user or computer description fields
- KeePass databases (if we are able to guess or crack the master password)
- Found on user systems and shares
- Files with names like `pass.txt`, `passwords.docx`, `passwords.xlsx` found on user systems, shares, and [Sharepoint](https://www.microsoft.com/en-us/microsoft-365/sharepoint/collaboration)
