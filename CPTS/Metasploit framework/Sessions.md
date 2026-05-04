#### Listing Active Sessions

```shell
msf6 exploit(windows/smb/psexec_psh) > sessions
```
#### Interacting with a Session

```shell
msf6 exploit(windows/smb/psexec_psh) > sessions -i 1 
[*] Starting interaction with 1... 

meterpreter >
```

## Jobs
#### Viewing the Exploit Command Help Menu

```shell
msf6 exploit(multi/handler) > exploit -h
```
#### Running an Exploit as a Background Job

```shell
msf6 exploit(multi/handler) > exploit -j
```
#### Listing Running Jobs

```shell
msf6 exploit(multi/handler) > jobs -l
```