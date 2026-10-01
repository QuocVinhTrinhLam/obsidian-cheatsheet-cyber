## [[ZEEK]]

![[Screenshot 2025-08-19 at 10.17.37.png]]

Sử dụng lệnh `zeek -C -r phishing.pcap` để lấy file.log
![[Screenshot 2025-08-19 at 10.18.22.png]]
![[Screenshot 2025-08-19 at 10.19.50.png]]

`cat conn.log | zeek-cut id.orig_h | sort | uniq -c`
![[Screenshot 2025-08-19 at 10.21.38.png]]

