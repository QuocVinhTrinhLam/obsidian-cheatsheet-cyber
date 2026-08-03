## Enumeration
|**Method**|**Description**|
|---|---|
|`Port Scanning`|ColdFusion typically uses port 80 for HTTP and port 443 for HTTPS by default. So, scanning for these ports may indicate the presence of a ColdFusion server. Nmap might be able to identify ColdFusion during a services scan specifically.|
|`File Extensions`|ColdFusion pages typically use ".cfm" or ".cfc" file extensions. If you find pages with these file extensions, it could be an indicator that the application is using ColdFusion.|
|`HTTP Headers`|Check the HTTP response headers of the web application. ColdFusion typically sets specific headers, such as "Server: ColdFusion" or "X-Powered-By: ColdFusion", that can help identify the technology being used.|
|`Error Messages`|If the application uses ColdFusion and there are errors, the error messages may contain references to ColdFusion-specific tags or functions.|
|`Default Files`|ColdFusion creates several default files during installation, such as "admin.cfm" or "CFIDE/administrator/index.cfm". Finding these files on the web server may indicate that the web application runs on ColdFusion.|
#### NMap ports and service scan results

```shell
3kjS@htb[/htb]$ nmap -p- -sC -Pn 10.129.247.30 --open
```
![](Screenshot%202026-08-02%20at%2020.19.11.png)

Navigating around the structure a bit shows lots of interesting info, from files with a clear `.cfm` extension to error messages and login pages.
![](Pasted%20image%2020260802201923.png)
![](Pasted%20image%2020260802201932.png)
![](Pasted%20image%2020260802201935.png)

The `/CFIDE/administrator` path, however, loads the ColdFusion 8 Administrator login page. Now we know for certain that `ColdFusion 8` is running on the server.
![](Pasted%20image%2020260802202155.png)
