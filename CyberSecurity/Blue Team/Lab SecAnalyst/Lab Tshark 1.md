## [[Tshark]] | [[A little Tshark]] | [[Regex for Forensic & Analyst]] | [[Lab  Tshark 2]]

___
### Quest 1:
```Terminal
tshark -r teamwork.pcap -Y "dns.qry.name" -T fields -e dns.qry.name -E header=y
```
![[Screenshot 2025-08-22 at 15.46.45.png]]
![[Screenshot 2025-08-22 at 15.51.28.png]]
___
### Quest 2:

```Terminal
tshark -r teamwork.pcap -Y "dns.qry.name" -T fields -e dns.qry.name -e ip.dst -e ip.src -e dns.a -E header=y
```
![[Screenshot 2025-08-22 at 15.53.45.png]]
___
### Quest 3:
![[Screenshot 2025-08-22 at 15.56.21.png]]
![[Screenshot 2025-08-22 at 15.59.43.png]]
```Terminal
grep -E "\w+@\w+\.\w+" fullcapture.txt 
```
![[Screenshot 2025-08-22 at 16.01.49.png]]
Defang -> `johnny5alive[at]gmail[.]com`