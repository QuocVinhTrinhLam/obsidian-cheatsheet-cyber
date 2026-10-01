# 🔐 SSH Tunneling Cheat Sheet

## 🧠 1. Tổng quan nhanh

- SSH tunneling = **đóng gói traffic qua SSH**
    
- Dùng để:
    
    - Bypass firewall / blacklist
        
    - Truy cập internal service
        
    - Pivot trong mạng nội bộ
        

---

## 🚪 2. Local Port Forwarding (SSH -L)

### 📌 Syntax

```bash
ssh -L [LOCAL_PORT]:[TARGET_IP]:[TARGET_PORT] user@ssh_server
```

### 📌 Ví dụ

```bash
ssh -i key.pem -L 8080:127.0.0.1:80 user@target
```

### 📌 Ý nghĩa

- Bạn truy cập:
    

```bash
http://localhost:8080
```

→ Thực chất đi tới:

```bash
target:127.0.0.1:80
```

### 🎯 Use case

- Truy cập web nội bộ
    
- Bypass IP blacklist (như case HTB Nibbles)
    
- Forward database (MySQL, PostgreSQL)
    

---

## 🔁 3. Remote Port Forwarding (SSH -R)

### 📌 Syntax

```bash
ssh -R [REMOTE_PORT]:[LOCAL_IP]:[LOCAL_PORT] user@ssh_server
```

### 📌 Ví dụ

```bash
ssh -R 4444:127.0.0.1:4444 user@target
```

### 📌 Ý nghĩa

- Target sẽ mở port 4444
    
- Traffic từ target → forward về máy bạn
    

### 🎯 Use case

- Reverse shell khi bạn bị NAT
    
- Expose local service ra ngoài
    

---

## 🧦 4. Dynamic Port Forwarding (SOCKS Proxy -D)

### 📌 Syntax

```bash
ssh -D [PORT] user@ssh_server
```

### 📌 Ví dụ

```bash
ssh -D 9050 user@target
```

### 📌 Dùng với proxychains

```bash
proxychains nmap -sT target_internal_ip
```

### 🎯 Use case

- Pivot toàn bộ traffic
    
- Scan mạng nội bộ (Nmap, Burp, browser)
    

---

## 🔄 5. Flow Visualization

### Local (-L)

```
[Your Machine] → localhost:PORT → SSH → Target:Internal Service
```

### Remote (-R)

```
[Target] → localhost:PORT → SSH → Your Machine
```

### Dynamic (-D)

```
[Your Tools] → SOCKS Proxy → SSH → Internal Network
```

---

## ⚙️ 6. Option quan trọng

|Option|Ý nghĩa|
|---|---|
|`-i key.pem`|Dùng private key|
|`-N`|Không mở shell (chỉ tunnel)|
|`-f`|Chạy background|
|`-C`|Enable compression|
|`-v`|Debug|

### 📌 Ví dụ tối ưu

```bash
ssh -i key.pem -N -f -L 8080:127.0.0.1:80 user@target
```

---

## 🧪 7. Combo thực chiến

### 🔹 Bypass blacklist brute force

```bash
ssh -L 8080:127.0.0.1:80 user@target
hydra http://localhost:8080/login
```

---

### 🔹 Pivot + scan internal

```bash
ssh -D 9050 user@target
proxychains nmap -sT 10.10.10.0/24
```

---

### 🔹 Forward database

```bash
ssh -L 3306:127.0.0.1:3306 user@target
mysql -h 127.0.0.1 -P 3306 -u root -p
```

---

## 🚨 8. Lỗi thường gặp

|Lỗi|Nguyên nhân|
|---|---|
|Connection refused|Service không chạy|
|Permission denied|Sai key / quyền|
|Port không bind được|Port đã dùng|
|Không truy cập được|Firewall / bind localhost|

---

## 🔐 9. Góc nhìn Blue Team

- Không trust `127.0.0.1`
    
- Disable unnecessary port forwarding:
    

```bash
AllowTcpForwarding no
```

- Monitor SSH logs
    
- Dùng Zero Trust thay vì IP-based trust
    

---

## 🎯 10. Key takeaway

- `-L` → vào trong (access internal)
    
- `-R` → đẩy ra ngoài (reverse access)
    
