## Cracking ZIP files

```shell
3kjS@htb[/htb]$ zip2john ZIP.zip > zip.hash
```

```shell
3kjS@htb[/htb]$ john --wordlist=rockyou.txt zip.hash

3kjS@htb[/htb]$ john zip.hash --show
```
## Cracking OpenSSL encrypted GZIP files

```shell
3kjS@htb[/htb]$ file GZIP.gzip
```

```shell
3kjS@htb[/htb]$ for i in $(cat rockyou.txt);do openssl enc -aes-256-cbc -d -in GZIP.gzip -k $i 2>/dev/null| tar xz;done
```
## Cracking BitLocker-encrypted drives

```shell
3kjS@htb[/htb]$ bitlocker2john -i Backup.vhd > backup.hashes 3kjS@htb[/htb]$ grep "bitlocker\$0" backup.hashes > backup.hash 3kjS@htb[/htb]$ cat backup.hash
```
#### Mounting BitLocker-encrypted drives in Windows
![[Pasted image 20260506171304.png]]
#### Mounting BitLocker-encrypted drives in Linux (or macOS)

```shell
3kjS@htb[/htb]$ sudo apt-get install dislocker
```

Next, we create two folders which we will use to mount the VHD.

```shell
3kjS@htb[/htb]$ sudo mkdir -p /media/bitlocker 
3kjS@htb[/htb]$ sudo mkdir -p /media/bitlockermount
```

We then use `losetup` to configure the VHD as [loop device](https://en.wikipedia.org/wiki/Loop_device), decrypt the drive using `dislocker`, and finally mount the decrypted volume:

```shell
3kjS@htb[/htb]$ sudo losetup -f -P Backup.vhd 
3kjS@htb[/htb]$ sudo dislocker /dev/loop0p2 -u1234qwer -- /media/bitlocker 
3kjS@htb[/htb]$ sudo mount -o loop /media/bitlocker/dislocker-file /media/bitlockermount
```

If everything was done correctly, we can now browse the files:

```shell
3kjS@htb[/htb]$ cd /media/bitlockermount/ 
3kjS@htb[/htb]$ ls -la
```

Once we have analyzed the files on the mounted drive, we can unmount it using the following commands:

```shell
3kjS@htb[/htb]$ sudo umount /media/bitlockermount 
3kjS@htb[/htb]$ sudo umount /media/bitlocker
```

