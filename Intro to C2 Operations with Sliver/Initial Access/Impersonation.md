## make-token

Let's dive in the `make-token` command and its [source code](https://github.com/BishopFox/sliver/blob/6443aaba3ddfc896ff5eb1bf0f2e5339064afe32/client/command/privilege/make-token.go).

```sh
sliver > help make-token

Command: make-token -u USERNAME -d DOMAIN -p PASSWORD
About: Creates a new Logon Session from the specified credentials and impersonate the resulting token.
You can specify a custon Logon Type using the --logon-type flag, which defaults to LOGON32_LOGON_NEW_CREDENTIALS.
Valid types are:

LOGON_INTERACTIVE
LOGON_NETWORK
LOGON_BATCH
LOGON_SERVICE
LOGON_UNLOCK
LOGON_NETWORK_CLEARTEXT
LOGON_NEW_CREDENTIALS


Usage:
======
  make-token [flags]

Flags:
======
  -d, --domain     string    domain of the user to impersonate
  -h, --help                 display help
  -T, --logon-type string    logon type to use (default: LOGON_NEW_CREDENTIALS)
  -p, --password   string    password of the user to impersonate
  -t, --timeout    int       command timeout in seconds (default: 60)
  -u, --username   string    username of the user to impersonate
```

|Type|Description|
|---|---|
|LOGON_INTERACTIVE|Type of logon mostly used by every user on the computer, be it using a console, RDP session or else.|
|LOGON_NETWORK|Type of logon intended for high performance servers that accept authenticators such as a Password, NT Hash, Kerberos Ticket|
|LOGON_BATCH|Type of logon equivalent to a Scheduled Task, Startup or else that could execute a process on behalf of the user|
|LOGON_SERVICE|Type of logon where the account must have service privileges enabled|
|LOGON_UNLOCK|Type of logon that uses [DLLs/GINA](https://learn.microsoft.com/en-us/windows/win32/secgloss/g-gly) that are loaded by Winlogon|
|LOGON_NETWORK_CLEARTEXT|Type of logon that utilizes plaintext credentials|
|LOGON_NEW_CREDENTIALS|Type of logon allow the current user to clone its token while specifying new set of credentials for outbound connections|
## Source Code

```go
<SNIP>
if _, ok := logonTypes[logonType]; !ok {
        con.PrintErrorf("Invalid logon type: %s\n", logonType)
        return
    }
<SNIP>
```

By default, if a `logonType` was not specified it will use `LOGON32_LOGON_NEW_CREDENTIALS` logon type. We can see this in the [priv_windows.go](https://github.com/BishopFox/sliver/blob/master/implant/sliver/priv/priv_windows.go#L243) source code.

```go
<SNIP>
func MakeToken(domain string, username string, password string, logonType uint32) error {
    var token windows.Token
    // Default to LOGON32_LOGON_NEW_CREDENTIALS
    if logonType == 0 {
        logonType = syscalls.LOGON32_LOGON_NEW_CREDENTIALS
    }
<SNIP>
```

