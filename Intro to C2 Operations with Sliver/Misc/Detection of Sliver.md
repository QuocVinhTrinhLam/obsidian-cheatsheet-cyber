#### Shell - Sigma rule breakdown

```yml
<SNIP>
logsource:
    category: process_creation
    product: windows
detection:
    selection:
        CommandLine|contains: '-NoExit -Command [Console]::OutputEncoding=[Text.UTF8Encoding]::UTF8'
    condition: selection
falsepositives:
    - Unlikely
level: critical
```

```go
var (
    // Shell constants
    commandPrompt = []string{"C:\\Windows\\System32\\cmd.exe"}
    powerShell    = []string{
        "C:\\Windows\\System32\\WindowsPowerShell\\v1.0\\powershell.exe",
        "-NoExit",
        "-Command", "[Console]::OutputEncoding=[Text.UTF8Encoding]::UTF8",
    }
)
```

|Command|Description|
|---|---|
|`-NoExit`|Keeps powershell running other command(s)|
|`-Command`|Executes the specified command(s)|
![](Detection%20of%20Sliver-20261119-124056.png)
#### Getsystem - high-level breakdown

By default, the `getsystem` command will inject itself into the `spoolsv.exe` process.

```sh
sliver > help getsystem

Spawns a new sliver session as the NT AUTHORITY\SYSTEM user (Windows Only)

Usage:
======
  getsystem [flags]

Flags:
======
  -h, --help              display help
  -p, --process string    SYSTEM process to inject into (default: spoolsv.exe)
  -t, --timeout int       command timeout in seconds (default: 60)
```

The default targeted process (`spoolsv.exe`) is closely monitored, as we can see in the article. However, if we change the process to `svchost.exe`, for example, it would not get detected, but it will get detected based on the `SeDebugPrivilege` being assigned to the process. Below is an image showcasing the `getsystem` targeting `svchost.exe`. As we can see, it got detected by Sysmon.

![](Detection%20of%20Sliver-20261119-124236.png)

Taking a high-level look at the source code for [priv_windows.go](https://github.com/BishopFox/sliver/blob/061166755d3ca195a3925e9016d2af6bd4f6af9d/implant/sliver/priv/priv_windows.go#L422) we can stumble upon the following function:

```go
// GetSystem starts a new RemoteTask in a SYSTEM owned process
func GetSystem(data []byte, hostingProcess string) (err error) {
    runtime.LockOSThread()
    defer runtime.UnlockOSThread()
    procs, _ := ps.Processes()
    for _, p := range procs {
        if p.Executable() == hostingProcess {
            err = SePrivEnable("SeDebugPrivilege")
            if err != nil {
                // {{if .Config.Debug}}
                log.Println("SePrivEnable failed:", err)
                // {{end}}
                return
            }
            err = taskrunner.RemoteTask(p.Pid(), data, false)
            if err != nil {
                // {{if .Config.Debug}}
                log.Println("RemoteTask failed:", err)
                // {{end}}
                return
            }
            break
        }
    }
    return
}
```

We also must be cautious as `getsystem` uses API calls monitored by the query:

```code
// SeDebugPrivilege constant used to identify if this privilege is enabled in Token
let SeDebugPriv = 1048576;
DeviceEvents
| where FileName == 'spoolsv.exe'
| where ActionType == 'CreateRemoteThreadApiCall'  <----
| where InitiatingProcessFileName !~ 'csrss.exe'
| project InitiatingProcessId, DeviceId, CreateTime=Timestamp, FileName
```
#### PsExec

```json
<SNIP>

sources:
  - name: Sliver PsExec - Services Registry Key
    query: |
        SELECT * FROM Artifact.Windows.System.Services() 
        WHERE Name =~ "^Sliver" or 
              DisplayName =~ "^Sliver" or 
              Description =~ "Sliver implant" or 
              PathName =~ ":\\\\Windows\\\\Temp\\\\[a-zA-Z0-9]{10}\\.exe"

  - name: Sliver PsExec - Service Installed Event Log
    query: |
        SELECT * FROM Artifact.Windows.EventLogs.EvtxHunter(PathRegex="System.evtx",IdRegex="^7045$")
        WHERE EventData.ServiceName =~ "^Sliver$" or 
             EventData.ImagePath =~ ":\\\\Windows\\\\Temp\\\\[a-zA-Z0-9]{10}\\.exe"
```

Let's look at the source code for [psexec.go](https://github.com/BishopFox/sliver/blob/061166755d3ca195a3925e9016d2af6bd4f6af9d/client/command/exec/psexec.go). Examining the following code snippet, we can see that `uploadPath` will take the path from `fmt.Sprintf` (and the hostname) and will convert the string to lowercase.

```go
var serviceBinary []byte
    profile, _ := cmd.Flags().GetString("profile")
    serviceName, _ := cmd.Flags().GetString("service-name")
    serviceDesc, _ := cmd.Flags().GetString("service-description")
    binPath, _ := cmd.Flags().GetString("binpath")
    customExe, _ := cmd.Flags().GetString("custom-exe")
    uploadPath := fmt.Sprintf(`\\%s\%s`, hostname, strings.ReplaceAll(strings.ToLower(binPath), "c:", "C$"))
```

However, the `uploadPath` and the `binPath` especially are hardcoded in [sliver.go](https://github.com/BishopFox/sliver/blob/061166755d3ca195a3925e9016d2af6bd4f6af9d/client/command/sliver.go#L674) line `674`.

```go
sliver.AddCommand(psExecCmd)
        Flags("", false, psExecCmd, func(f *pflag.FlagSet) {
            f.StringP("service-name", "s", "Sliver", "name that will be used to register the service")
            f.StringP("service-description", "d", "Sliver implant", "description of the service")
            f.StringP("profile", "p", "", "profile to use for service binary")
            f.StringP("binpath", "b", "c:\\windows\\temp", "directory to which the executable will be uploaded")
            f.StringP("custom-exe", "c", "", "custom service executable to use instead of generating a new Sliver")

            f.Int64P("timeout", "t", defaultTimeout, "grpc timeout in seconds")
        })
        FlagComps(psExecCmd, func(comp *carapace.ActionMap) {
            (*comp)["custom-exe"] = carapace.ActionFiles()
        })
        carapace.Gen(psExecCmd).PositionalCompletion(carapace.ActionValues().Usage("hostname (required)"))
```

This means that every time we execute `psexec` the binary will be uploaded to `c:\\windows\\temp`. However, in theory, we could alter `binpath` to a different location using the `-b`/`--binpath` parameter. Moreover, should we remember the VQL, it will scrutinize the path and filename using the regex pattern `:\\\\Windows\\\\Temp\\\\[a-zA-Z0-9]{10}\\.exe`. The functions responsible for generating the name again can be found in [psexec.go](https://github.com/BishopFox/sliver/blob/061166755d3ca195a3925e9016d2af6bd4f6af9d/client/command/exec/psexec.go#L186).

```go
func randomString() string {
    alphanumeric := "abcdefghijklmnopqrstuvwxyz0123456789"
    str := ""
    for index := 0; index < insecureRand.Intn(8)+1; index++ {
        str += string(alphanumeric[insecureRand.Intn(len(alphanumeric))])
    }
    return str
}

func randomFileName() string {
    noun := randomString()
    noun = strings.ToLower(noun)
    switch insecureRand.Intn(3) {
    case 0:
        noun = strings.ToUpper(noun)
    case 1:
        noun = strings.ToTitle(noun)
    }

    separators := []string{"", "", "", "", "", ".", "-", "_", "--", "__"}
    sep := separators[insecureRand.Intn(len(separators))]

    alphanumeric := "abcdefghijklmnopqrstuvwxyz0123456789"
    prefix := ""
    for index := 0; index < insecureRand.Intn(3); index++ {
        prefix += string(alphanumeric[insecureRand.Intn(len(alphanumeric))])
    }
    suffix := ""
    for index := 0; index < insecureRand.Intn(6)+1; index++ {
        suffix += string(alphanumeric[insecureRand.Intn(len(alphanumeric))])
    }

    return fmt.Sprintf("%s%s%s%s%s", prefix, sep, noun, sep, suffix)
}
```
