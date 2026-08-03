# [WordPress - Discovery & Enumeration](WordPress%20-%20Discovery%20&%20Enumeration.md)
### Enumerate the host and find a flag.txt flag in an accessible directory.

```shell
gobuster dir -u http://blog.inlanefreight.local/wp-content/ -w /usr/share/seclists/Discovery/DNS/subdomains-top1million-5000.txt
```
![](Screenshot%202026-07-27%20at%2013.59.09.png)
![](Screenshot%202026-07-27%20at%2014.03.03.png)
### Perform manual enumeration to discover another installed plugin. Submit the plugin name as the answer (3 words).
![](Screenshot%202026-07-27%20at%2014.09.21.png)
`wp-sitemap-page`
### Find the version number of this plugin. (i.e., 4.5.2)

`http://blog.inlanefreight.local/wp-content/plugins/wp-sitemap-page/readme.txt`
![](Screenshot%202026-07-27%20at%2014.20.52.png)