
---

## 🌐 1. Application Layer Protocols

|Protocol|Port mặc định|Mô tả|Use Case|
|---|---|---|---|
|**HTTP**|80|Truyền tải web không mã hóa|Web cơ bản|
|**HTTPS**|443|HTTP + SSL/TLS, mã hóa|Web an toàn|
|**FTP**|21|Truyền file, không bảo mật|Chuyển file (giờ ít dùng, thay bằng SFTP/FTPS)|
|**SFTP**|22 (qua SSH)|Truyền file an toàn|Upload/download file|
|**SMTP**|25|Gửi email|Mail server gửi đi|
|**IMAP**|143 (993 TLS)|Đọc email, giữ bản gốc trên server|Gmail, Outlook|
|**POP3**|110 (995 TLS)|Đọc email, tải về máy|Mail client cũ|
|**DNS**|53|Phân giải tên miền ↔ IP|Truy cập web|
|**DHCP**|67/68|Cấp phát IP tự động|Kết nối mạng dễ dàng|
|**SNMP**|161|Quản lý thiết bị mạng|Monitoring routers/switches|
|**SSH**|22|Remote login bảo mật|Quản lý server từ xa|
|**Telnet**|23|Remote login **không mã hóa**|(lỗi thời, dễ sniff)|

---

## 📦 2. Transport Layer

|Protocol|Port|Mô tả|Use Case|
|---|---|---|---|
|**TCP**|-|Kết nối hướng phiên, đáng tin cậy|Web, mail, SSH|
|**UDP**|-|Không kết nối, nhanh, không đảm bảo|Streaming, VoIP, DNS|
|**TLS/SSL**|443 (thường)|Mã hóa kênh TCP|HTTPS, SMTPS, IMAPS|

---

## 🌉 3. Network Layer

|Protocol|Port|Mô tả|Use Case|
|---|---|---|---|
|**IP (IPv4/IPv6)**|-|Định danh host trên mạng|Giao tiếp internet|
|**ICMP**|-|Trao đổi thông tin điều khiển|`ping`, `traceroute`|
|**ARP**|-|Ánh xạ IP ↔ MAC|Giao tiếp trong LAN|

---

## 🖧 4. Data Link & Physical Layer

- **Ethernet (802.3)**: Chuẩn truyền dữ liệu có dây.
    
- **Wi-Fi (802.11)**: Chuẩn mạng không dây.
    
- **PPP**: Point-to-Point Protocol, hay dùng trong VPN.
    
- **Bluetooth (802.15.1)**: Kết nối tầm ngắn.
    

---

## 🔥 Protocol trong Cybersecurity (hay gặp trong Pentest & Blue Team)

- **SMB (445)** → file sharing Windows, hay bị khai thác (EternalBlue).
    
- **RDP (3389)** → remote desktop, thường bị brute-force.
    
- **LDAP (389/636)** → quản lý directory (Active Directory).
    
- **NTP (123)** → đồng bộ thời gian, có thể bị lợi dụng DDoS.
    
- **TFTP (69)** → lightweight FTP, hay bị lộ config.
    
- **IKE/IPSec (500, 4500)** → VPN.
    
- **Kerberos (88)** → authentication trong Windows domain.
    

---

👉 Tips học nhanh:

- Nhớ **port quen thuộc** (22 SSH, 80 HTTP, 443 HTTPS, 445 SMB, 3389 RDP).
    
- Chia theo nhóm **Web – File Transfer – Email – Remote – Directory**.
    
- Liên hệ luôn tới **lab pentest**: `nmap -sV -p- target` để thấy ngay service chạy protocol nào.
    

---
