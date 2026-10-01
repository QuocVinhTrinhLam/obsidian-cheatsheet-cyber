# Scenario

1. You must first identify the hash type and then crack the hash and provide the cleartext value.

```
0c67ac18f50c5e6b9398bfe1dc3e156163ba10ef
```

2. Crack the provided NetNTLMv2 hash to help them proceed.

```
bjones::INLANEFREIGHT:699f1e768bd69c00:5304B6DB9769D974A8F24C4F4309B6BC:0101000000000000C0653150DE09D2010409DF59F277926E000000000200080053004D004200330001001E00570049004E002D00500052004800340039003200520051004100460056000400140053004D00420033002E006C006F00630061006C000300340057004900<SNIP>
```

3. One of the Kerberos TGS tickets retrieved is for a user that is a member of the Local Administrators group on one server. Can you help them crack this hash and move laterally to this server?

```
$krb5tgs$23$*sql_svc$INLANEFREIGHT.LOCAL$mssql/inlanefreight.local~1443*$80be357f5e68b4f64a185397bf72cf1c$579d028f0f91f5791683844c3a03f48972cb9013eddf11728fc19500679882106538379cbe1cb503677b757bcb56f9e708cd173a5b04ad8fc462fa380ffcb4e1b4feed820fa183109e2653b46faee565af76d5b0ee8460a3e0ad48ea098656584de67974372f4a762daa625fb292556b407c0c97fd629506bd558dede0c950a039e2e3433ee956fc218a54b148d3e5d1f99781ad4e419bc76632e30ea2c1660663ba9866230c790ba166b865d5153c6b85a184fbafd5a4af6d3200d67857da48e20039bbf31853da46215cbbc5ebae6a3b0225b6651ec8cc792c8c3d5893a8d014f9d297ac297288e76d27a1ed2942d6997f6b24198e64fea9ff5a94badd53cc12a73e9505e4dab36e4bd1ef7fe5a08e527d9046b49e730d83d8af395f06fe35d360c59ab8ebe2c3b7553acf8d40c296b86c1fb26fdf<SNIP>
```

4. This hash is typically very difficult to crack but, if successful, will grant full administrative control over the entire Active Directory Environment.

```
$DCC2$10240#backup_admin#62dabbde52af53c75f37df260af1008e
```
___
### What type of hash did your colleague obtain from the SQL injection attack?

Firstly, we have to identify the hash by using hashid

![](Screenshot%202026-10-01%20at%2008.37.46.png)
### What is the cleartext password for the hash obtained from SQL injection in example 1?

We saw that hash is SHA-1 we can use mode 100

```sh
hashcat -a 0 -m 100 0c67ac18f50c5e6b9398bfe1dc3e156163ba10ef /usr/share/wordlists/rockyou.txt
```

![](Screenshot%202026-10-01%20at%2008.42.13.png)
### What is the cleartext password value for the NetNTLMv2 hash?

Using mode 5600 for this NetNTLMv2 hash

```sh
hashcat -a 0 -m 5600 ntlm_hash /usr/share/wordlists/rockyou.txt
```

![](Screenshot%202026-10-01%20at%2008.45.41.png)
### Crack the TGS ticket obtained from the Kerberoasting attack.

For searching google to crack the TGS ticket we have to use mode 13100

```sh
hashcat -a 0 -m 13100 TGS_ticket /usr/share/wordlists/rockyou.txt
```

![](Screenshot%202026-10-01%20at%2008.48.06.png)
### What is the cleartext password value for the MS Cache 2 hash?

```sh
hashcat -a 0 -m 2100 mscache /usr/share/wordlists/rockyou.txt
```

![](Screenshot%202026-10-01%20at%2008.51.01.png)
### After cracking the NTLM password hashes contained in the NTDS.dit file, perform an analysis of the results and find out the MOST common password in the INLANEFREIGHT.LOCAL domain.

Firstly, count the frequency of duplicate hashes in the original NTDS file

```sh
cut -d: -f4 DC01.inlanefreight.local.ntds | sort | uniq -c | sort -nr | head
```

![](Screenshot%202026-10-01%20at%2009.05.42.png)

Secondly, crack the hash using mode 1000 (NTLM)

```sh
hashcat -a 0 -m 1000 db3a9af5e74be03220d213b47ef25b53 /usr/share/wordlists/rockyou.txt --show
```

![](Screenshot%202026-10-01%20at%2009.06.52.png)