# [Application Discovery & Enumeration](Application%20Discovery%20&%20Enumeration.md)
### Use what you've learned from this section to generate a report with EyeWitness. What is the name of the .db file EyeWitness creates in the inlanefreight_eyewitness folder? (Format: filename.db)

Add these Vhosts to the `scopelists`
![](Screenshot%202026-07-27%20at%2011.57.43.png)

Then run the `nmap`
```shell
sudo nmap 10.129.42.195 -p 80,443,8000,8080,8180,8888,10000 --open -oA web_discovery -iL scope_list
```

```shell
python3 EyeWitness.py --web -x ~/web_discovery.xml -d inlanefreight_eyewitness
```
![](Screenshot%202026-07-27%20at%2011.53.47.png)
![](Screenshot%202026-07-27%20at%2011.54.11.png)
### What does the header on the title page say when opening the aquatone_report.html page with a web browser? (Format: 3 words, case sensitive)

```shell
cat web_discovery.xml | ./aquatone -nmap
```
![](Screenshot%202026-07-27%20at%2011.56.41.png)
