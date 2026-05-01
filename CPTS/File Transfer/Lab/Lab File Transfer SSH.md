# [[Linux File Transfer Method]] 

```Shell
wget https://academy.hackthebox.com/storage/modules/24/upload_nix.zip
```

```Shell
unzip upload_nix.zip
```

```Shell
md5sum upload_nix.txt
```

```
base64 upload_nix.txt > base64encoded.txt
```

Next, i'll open http server from Pwnbox 

```Shell
┌─[us-academy-5]─[10.10.14.56]─[htb-ac-1951749@htb-wlqvipwax9]─[~]
└──╼ [★]$ python3 -m http.server
```
![[Screenshot 2026-05-01 at 11.12.33.png]]

Turn to victim and get the `base64encoded.txt` by using this command below:

```Shell
ssh htb-student@10.129.234.168
```

```Shell
wget <ip:port>/base64encoded.txt
```

Turn the .txt to the zip 

```Shell
htb-student@nix04:~$ echo 'MDQ4MDkwYmM3ZWQwNGY3NTg2NTg5NzVkZjhmODYyYzg=' | base64 -d > haha.zip
```

```Shell
htb-student@nix04:~$ echo 'MDQ4MDkwYmM3ZWQwNGY3NTg2NTg5NzVkZjhmODYyYzg=' | base64 -d > haha.zip
```

Then i got the flag `159cfe5c65054bbadb2761cfa359c8b0`
