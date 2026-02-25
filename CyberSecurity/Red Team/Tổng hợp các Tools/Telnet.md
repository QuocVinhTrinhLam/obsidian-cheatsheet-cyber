
| **Protocol** | **TCP Port** | **Application(s)** | **Data Security** |
| ------------ | ------------ | ------------------ | ----------------- |
| FTP          | 21           | File Transfer      | Cleartext         |
| HTTP         | 80           | Worldwide Web      | Cleartext         |
| IMAP         | 143          | Email (MDA)        | Cleartext         |
| POP3         | 110          | Email (MDA)        | Cleartext         |
| SMTP         | 25           | Email (MTA)        | Cleartext         |
| Telnet       | 23           | Remote Access      | Cleartext         |

## 🧠 MỤC TIÊU

- Kiểm tra xem dịch vụ có hoạt động không
    
- Tìm hiểu cách phản hồi của server
    
- Khai thác lỗ hổng cấu hình sai
    
- Gửi yêu cầu thủ công → tìm bug, dữ liệu nhạy cảm


---

## 🧰 CÚ PHÁP CƠ BẢN

```bash
telnet <target_ip/domain> <port>
```

---

## 🌐 HTTP (port 80)

```bash
telnet target.com 80
GET / HTTP/1.1
Host: target.com
```

🎯 Mục đích:

- Kiểm tra phản hồi server
    
- Phân tích headers (Server, Set-Cookie,...)
    
- Kiểm tra lỗi cấu hình
    

---

## 📧 SMTP (port 25)

```bash
telnet target.com 25
HELO attacker.com
MAIL FROM:<attacker@evil.com>
RCPT TO:<victim@target.com>
DATA
Subject: Hello
This is a test mail
.
```

🎯 Mục đích:

- Gửi mail giả mạo
    
- Kiểm tra open relay
    
- Dò người dùng qua:
    
    ```bash
    VRFY admin
    EXPN admin
    ```
    

---

## 📥 POP3 (port 110)

```bash
telnet target.com 110
USER victim
PASS 123456
LIST
RETR 1
```

🎯 Mục đích:

- Thử brute-force
    
- Đọc email chứa token, mã OTP
    
- Thu thập dữ liệu nhạy cảm
    

---

## 📬 IMAP (port 143)

```bash
telnet target.com 143
a LOGIN victim 123456
a SELECT INBOX
a FETCH 1 BODY[]
```

🎯 Mục đích:

- Đọc metadata, nội dung email
    
- Truy cập nhiều folder hơn POP3
    
- Thu thập email bị bỏ quên
    

---

## 📁 FTP (port 21)

```bash
telnet target.com 21
USER anonymous
PASS anything
```

🎯 Mục đích:

- Kiểm tra login ẩn danh
    
- Tìm lỗi upload file
    
- Ghi file độc vào thư mục web
    

---

## 🔌 TELNET SERVICE (port 23)

```bash
telnet target.com 23
```

🎯 Mục đích:

- Kiểm tra banner OS
    
- Đăng nhập thử mật khẩu mặc định
    
- Lấy shell nếu không có xác thực
    

---

## 🧨 Kỹ thuật mở rộng

|Kỹ thuật|Giải thích|
|---|---|
|Banner Grabbing|Dò thông tin server qua phản hồi ban đầu|
|Manual Exploitation|Tự gửi lệnh để phân tích lỗi dịch vụ|
|Email Enumeration|`VRFY`, `EXPN`, `RCPT TO` giúp xác định user|
|LFI test|`GET /?page=../../etc/passwd HTTP/1.1`|

---

## 🧩 GỢI Ý CÔNG CỤ HỖ TRỢ

|Tên|Mục đích|
|---|---|
|`nc` (netcat)|Mạnh hơn Telnet, thay thế linh hoạt|
|Wireshark|Sniff Telnet session để phân tích|
|Burp Suite|Nếu muốn thực hiện tương tác HTTP dễ hơn|
|Metasploit|Tự động hóa các khai thác với Telnet / SMTP modules|

---

