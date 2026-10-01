#### Syntax

```shellsession
<No.> <type>/<os>/<service>/<name>
```

#### Example

```shell
794 exploit/windows/ftp/scriptftp_list
```

#### Type

|**Type**|**Description**|
|---|---|
|`Auxiliary`|Scanning, fuzzing, sniffing, and admin capabilities. Offer extra assistance and functionality.|
|`Encoders`|Ensure that payloads are intact to their destination.|
|`Exploits`|Defined as modules that exploit a vulnerability that will allow for the payload delivery.|
|`NOPs`|(No Operation code) Keep the payload sizes consistent across exploit attempts.|
|`Payloads`|Code runs remotely and calls back to the attacker machine to establish a connection (or shell).|
|`Plugins`|Additional scripts can be integrated within an assessment with `msfconsole` and coexist.|
|`Post`|Wide array of modules to gather information, pivot deeper, etc.|

#### MSF - Specific Search

```shell
msf6 > search type:exploit platform:windows cve:2021 rank:excellent microsoft
```

