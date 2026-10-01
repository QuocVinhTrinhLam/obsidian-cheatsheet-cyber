# Server Message Block (SMB)
## Windows
![[Pasted image 20260513200849.png]]
![[Pasted image 20260513200853.png]]
![[Pasted image 20260513200857.png]]
#### Windows CMD - DIR

```cmd
C:\htb> dir \\192.168.220.129\Finance\
```
#### Windows CMD - Net Use

```cmd
C:\htb> net use n: \\192.168.220.129\Finance
```

We can also provide a username and password to authenticate to the share.

```cmd
C:\htb> net use n: \\192.168.220.129\Finance /user:plaintext Password123
```
#### Windows CMD - DIR

```cmd
C:\htb> dir n: /a-d /s /b | find /c ":\" 
29302
```

| **Syntax** | **Description**                                                |
| ---------- | -------------------------------------------------------------- |
| `dir`      | Application                                                    |
| `n:`       | Directory or drive to search                                   |
| `/a-d`     | `/a` is the attribute and `-d` means not directories           |
| `/s`       | Displays files in a specified directory and all subdirectories |
| `/b`       | Uses bare format (no heading information or summary)           |

```cmd
C:\htb>dir n:\*cred* /s /b 

n:\Contracts\private\credentials.txt 

C:\htb>dir n:\*secret* /s /b 

n:\Contracts\private\secret.txt
```
#### Windows CMD - Findstr

```cmd
c:\htb>findstr /s /i cred n:\*.*
```
### Windows PowerShell
#### Windows PowerShell

```cmd
PS C:\htb> Get-ChildItem \\192.168.220.129\Finance\
```

```cmd
PS C:\htb> New-PSDrive -Name "N" -Root "\\192.168.220.129\Finance" -PSProvider "FileSystem"
```
#### Windows PowerShell - PSCredential Object

```Powershell
PS C:\htb> $username = 'plaintext' 

PS C:\htb> $password = 'Password123' 

PS C:\htb> $secpassword = ConvertTo-SecureString $password -AsPlainText -Force 

PS C:\htb> $cred = New-Object System.Management.Automation.PSCredential $username, $secpassword 

PS C:\htb> New-PSDrive -Name "N" -Root "\\192.168.220.129\Finance" -PSProvider "FileSystem" -Credential $cred
```
#### Windows PowerShell - GCI

```Powershell
PS C:\htb> N: PS N:\> (Get-ChildItem -File -Recurse | Measure-Object).Count 29302
```

```Powershell
PS C:\htb> Get-ChildItem -Recurse -Path N:\ -Include *cred* -File
```
#### Windows PowerShell - Select-String

```Powershell
PS C:\htb> Get-ChildItem -Recurse -Path N:\ | Select-String "cred" -List
```
## Linux
#### Linux - Mount

```shell
3kjS@htb[/htb]$ sudo mkdir /mnt/Finance 

3kjS@htb[/htb]$ sudo mount -t cifs -o username=plaintext,password=Password123,domain=. //192.168.220.129/Finance /mnt/Finance
```

```shell
3kjS@htb[/htb]$ mount -t cifs //192.168.220.129/Finance /mnt/Finance -o credentials=/path/credentialfile
```
#### CredentialFile

```shell
username=plaintext password=Password123 domain=.
```
#### Linux - Find

```shell
3kjS@htb[/htb]$ find /mnt/Finance/ -name *cred* 

/mnt/Finance/Contracts/private/credentials.txt
```

```shell
3kjS@htb[/htb]$ grep -rn /mnt/Finance/ -ie cred 

/mnt/Finance/Contracts/private/credentials.txt:1:admin:SecureCredentials! /mnt/Finance/Contracts/private/secret.txt:1:file with all credentials
```
## Other Services
### Email
#### Linux - Install Evolution

```shell
3kjS@htb[/htb]$ sudo apt-get install evolution 
...SNIP...
```
### Video - Connecting to IMAP and SMTP using Evolution
![[Pasted image 20260513201530.png]]
### Databases
![[Pasted image 20260513201542.png]]
## Command Line Utilities
#### Linux - SQSH

```shell
3kjS@htb[/htb]$ sqsh -S 10.129.20.13 -U username -P Password123
```
#### Windows - SQLCMD

```cmd
C:\htb> sqlcmd -S 10.129.20.13 -U username -P Password123
```
### MySQL
#### Linux - MySQL

```shell
3kjS@htb[/htb]$ mysql -u username -pPassword123 -h 10.129.20.13
```
#### Windows - MySQL

```cmd
C:\htb> mysql.exe -u username -pPassword123 -h 10.129.20.13
```
#### Install dbeaver

```shell
3kjS@htb[/htb]$ sudo dpkg -i dbeaver-<version>.deb
```
#### Run dbeaver

```shell
3kjS@htb[/htb]$ dbeaver &
```
