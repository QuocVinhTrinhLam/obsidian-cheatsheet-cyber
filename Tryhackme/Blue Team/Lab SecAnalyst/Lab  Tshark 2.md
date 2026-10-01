## [[Tshark]] | [[A little Tshark]] | [[Lab Tshark 1]]
___
# Quest 1
![[Screenshot 2025-08-23 at 12.27.01.png]]
```Terminal
tshark -r directory-curiosity.pcap -Y "dns" -T fields -e "dns.qry.name"
```

# Quest 2
![[Screenshot 2025-08-23 at 12.29.48.png]]
```Terminal
tshark -r directory-curiosity.pcap -Y "http.request and http.host contains jx2-bavuong.com" | nl
```

# Quest 3
![[Screenshot 2025-08-23 at 12.32.43.png]]
```Terminal
tshark -r directory-curiosity.pcap -Y "dns.qry.name contains jx2-bavuong.com" -T fields -e"dns.a"
```

# Quest 4
![[Screenshot 2025-08-23 at 12.35.28.png]]
```Terminal
tshark -r directory-curiosity.pcap  -T fields -e "http.server" | awk NF
```

# Quest 5
![[Screenshot 2025-08-23 at 12.41.24.png]]
```Terminal
tshark -r directory-curiosity.pcap -Y "tcp.stream eq 0" -z follow,tcp,ascii,0
```

# Quest 6, 7, 8 (cùng quest terminal 5)

# Quest 9
![[Screenshot 2025-08-23 at 12.45.51.png]]
Xuất file http ra output bằng câu lệnh `tshark -r directory-curiosity.pcap --export-objects http,./output` rồi `sha256sum vlauto.exe`

# Quest 10
![[Screenshot 2025-08-23 at 12.47.28.png]]

# Quest 11
![[Screenshot 2025-08-23 at 12.48.31.png]]
Phần **Behaviour** của VirusTotal