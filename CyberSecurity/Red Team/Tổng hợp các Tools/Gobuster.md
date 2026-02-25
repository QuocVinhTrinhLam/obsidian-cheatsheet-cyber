### 📁 **1. Directory Bruteforcing (quét thư mục)**

bash

`gobuster dir -u http://target.com -w /path/to/wordlist.txt`

📌 Thêm tùy chọn quan trọng:

|Tùy chọn|Mô tả|
|---|---|
|`-u`|URL đích (target)|
|`-w`|Wordlist để quét|
|`-x php,html,txt`|Thêm phần mở rộng tệp để kiểm tra|
|`-t 50`|Số luồng song song (default 10)|
|`-o result.txt`|Ghi kết quả ra file|
|`--no-error`|Ẩn các lỗi kết nối/HTTP error|
|`-s 200,204,301,302`|Chỉ hiện kết quả có mã trạng thái cụ thể|
### 🌐 **2. DNS Subdomain Bruteforce (quét subdomain)**

bash

`gobuster dns -d target.com -w /path/to/wordlist.txt`

📌 Tùy chọn thêm:

|Tùy chọn|Mô tả|
|---|---|
|`-d`|Domain đích|
|`-w`|Wordlist chứa tên subdomain|
|`-o`|Lưu kết quả ra file|
|`--wildcard`|Dùng nếu gặp DNS wildcard (mọi subdomain trả cùng IP)|
|`-t`|Số luồng quét song song|

📍 **Ví dụ:**

bash

`gobuster dns -d example.com -w /usr/share/seclists/Discovery/DNS/subdomains-top1million-5000.txt --wildcard`

---

### 📂 **3. VHOST Bruteforce (quét virtual host)**

bash

`gobuster vhost -u http://target.com -w /path/to/wordlist.txt`

📌 Tùy chọn thêm:

|Tùy chọn|Mô tả|
|---|---|
|`-u`|URL/IP đích|
|`-w`|Wordlist chứa các vhost name|
|`-t`|Số luồng quét|
|`-o`|Ghi kết quả|

📍 **Ví dụ:**

bash

`gobuster vhost -u http://10.10.10.10 -w /usr/share/wordlists/vhosts.txt -t 30`

---

### 📜 **4. Một số wordlist hay dùng**

| Mục đích  | Đường dẫn trong Kali Linux                                          |
| --------- | ------------------------------------------------------------------- |
| Thư mục   | `/usr/share/wordlists/dirb/common.txt`                              |
| Subdomain | `/usr/share/seclists/Discovery/DNS/subdomains-top1million-5000.txt` |
| VHOST     | `/usr/share/seclists/Discovery/DNS/namelist.txt`                    |

---

### ⚠️ **Tips & Lưu ý**

- Gobuster **chỉ hỗ trợ HTTP/HTTPS** — không dùng cho FTP, SSH.
    
- Luôn kiểm tra **wildcard DNS** trước khi quét subdomain.
    
- Kết hợp với **Burp Suite** để xác định false positive/negative từ kết quả.