
---

## 1. CEH Attack Lifecycle (Rất hay ra)

- **Reconnaissance**
    
    - Passive: OSINT, Google dorking, WHOIS
        
    - Active: Ping, DNS query, banner grabbing
        
- **Scanning**
    
    - Discover hosts, ports, services
        
    - Tools: Nmap, Masscan
        
- **Enumeration**
    
    - Extract **usernames, shares, system info**
        
    - Tools: enum4linux, SNMPwalk, NetBIOS
        
- **Gaining Access**
    
    - Exploit vulnerabilities
        
- **Maintaining Access**
    
    - Backdoor, persistence
        
- **Covering Tracks**
    
    - Clear logs, timestomp
        

👉 Keyword nhớ: _Enumeration = users & internal details_

---

## 2. Nmap Quick Mapping

- `-sS` → SYN (stealth, half-open)
    
- `-sT` → TCP Connect (full handshake)
    
- `-sV` → Service version
    
- `-O` → OS detection
    
- `-A` → Aggressive (OS + version + scripts)
    

👉 Stealth = SYN scan

---

## 3. TCP / Network Fundamentals

### TCP 3-way handshake

1. Client → SYN
    
2. Server → **SYN-ACK**
    
3. Client → ACK
    

### Protocol weaknesses

- ARP → spoofing (no auth)
    
- ICMP → ping sweep
    
- DNS → zone transfer
    

---

## 4. Web Attacks – Phân biệt nhanh

### XSS

- Inject script
    
- Chạy trên browser victim
    
- Hay qua User-Agent, Referer
    

### CSRF

- Force browser gửi request
    
- Dựa vào session đã login
    

### SQL Injection

- Error-based → lỗi visible
    
- Union-based → trả data
    
- Blind time-based → delay response
    

👉 Authenticated request = CSRF

---

## 5. Password Attacks

- **Hydra** → Online brute-force (SSH, FTP, HTTP)
    
- **John / Hashcat** → Offline hash cracking
    
- **Rainbow table** → Tăng tốc crack (precomputed hash)
    

---

## 6. Metasploit Concept Mapping

- Exploit → Cách vào
    
- Payload → Làm gì sau khi vào
    
- Post module → Maintain access, gather info
    
- Auxiliary → Scan, fuzz, enum
    

👉 Maintain access = Post

---

## 7. Malware Classification (CEH rất thích hỏi)

- **Virus** → cần host file
    
- **Worm** → tự lây lan
    
- **Trojan** → giả dạng hợp pháp
    
- **Rootkit** → ẩn sự tồn tại
    

👉 Self-propagating = Worm

---

## 8. Wireless Security

- **WEP** → RC4 + IV yếu (broken)
    
- WPA → cải tiến
    
- WPA2 → AES
    
- WPA3 → secure nhất
    

👉 RC4 yếu = WEP

---

## 9. Social Engineering

- Phishing → gửi mồi hàng loạt
    
- **Pretexting** → giả danh authority + kịch bản
    
- Tailgating → theo người khác vào
    
- Shoulder surfing → nhìn lén
    

👉 Authority + scenario = Pretexting

---

## 10. Defensive Controls (BEST / MOST)

- Brute-force → Account lockout
    
- Data in transit → TLS
    
- Data at rest → Disk / DB encryption
    
- Detection → Logs, IDS
    

---

## 11. CEH Exam Mindset (Cực quan trọng)

- Chọn **BEST / MOST / PRIMARY**
    
- Không chọn đáp án “hacker hay dùng”
    
- Chọn đáp án **đúng định nghĩa sách**
    

---
