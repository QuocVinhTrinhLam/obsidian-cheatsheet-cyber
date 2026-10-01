___
### **Network for RED (NMAP + DIG,NSLOOKUP,…)**

![[535841084_122255255432195962_9197601690488015739_n.jpg]]

| -sL                                                             | List scan – list targets without scanning                                                          |
| --------------------------------------------------------------- | -------------------------------------------------------------------------------------------------- |
| **_Host Discovery_**                                            |                                                                                                    |
| -sn                                                             | Ping scan – host discovery only                                                                    |
| **_Port Scanning_**                                             |                                                                                                    |
| -sT                                                             | TCP connect scan – complete three-way handshake                                                    |
| -sS                                                             | TCP SYN – only first step of the three-way handshake                                               |
| -sU                                                             | UDP Scan                                                                                           |
| -F                                                              | Fast mode – scans the 100 most common ports                                                        |
| -p[range]                                                       | Specifies a range of port numbers – -p- scans all the ports                                        |
| -Pn                                                             | Treat all hosts as online – scan hosts that appear to be down                                      |
| **_Service Detection_**                                         |                                                                                                    |
| -O                                                              | OS detection                                                                                       |
| -sV                                                             | Service version detection                                                                          |
| -A                                                              | OS detection, version detection, and other additions                                               |
| **_Timing_**                                                    |                                                                                                    |
| -T<0-5>                                                         | Timing template – paranoid (0), sneaky (1), polite (2), normal (3), aggressive (4), and insane (5) |
| --min-parallelism <numprobes> and --max-parallelism <numprobes> | Minimum and maximum number of parallel probes                                                      |
| --min-rate <number> and --max-rate <number>                     | Minimum and maximum rate (packets/second)                                                          |
| --host-timeout                                                  | Maximum amount of time to wait for a target host                                                   |
| **_Real-time output_**                                          |                                                                                                    |
| -v                                                              | Verbosity level – for example, -vv and -v4                                                         |
| -d                                                              | Debugging level – for example -d and -d9                                                           |
| **_Report_**                                                    |                                                                                                    |
| -oN <filename>                                                  | Normal output                                                                                      |
| -oX <filename>                                                  | XML output                                                                                         |
| -oG <filename>                                                  | grep-able output                                                                                   |
| -oA <basename>                                                  | Output in all major formats                                                                        |

| **Scan Type**          | **Example Command**                       |
| ---------------------- | ----------------------------------------- |
| ARP Scan               | sudo nmap -PR -sn MACHINE_IP/24           |
| ICMP Echo Scan         | sudo nmap -PE -sn MACHINE_IP/24           |
| ICMP Timestamp Scan    | sudo nmap -PP -sn MACHINE_IP/24           |
| ICMP Address Mask Scan | sudo nmap -PM -sn MACHINE_IP/24           |
| TCP SYN Ping Scan      | sudo nmap -PS22,80,443 -sn MACHINE_IP/30  |
| TCP ACK Ping Scan      | sudo nmap -PA22,80,443 -sn MACHINE_IP/30  |
| UDP Ping Scan          | sudo nmap -PU53,161,162 -sn MACHINE_IP/30 |

Remember to add -sn if you are only interested in host discovery without port-scanning. Omitting -sn will let Nmap default to port-scanning the live hosts.


| **Option** | **Purpose**                      |
| ---------- | -------------------------------- |
| -n         | no DNS lookup                    |
| -R         | reverse-DNS lookup for all hosts |
| -sn        | host discovery only              |


| **Port Scan Type**             | **Example Command**                                  |
| ------------------------------ | ---------------------------------------------------- |
| TCP Null Scan                  | sudo nmap -sN 10.10.167.5                            |
| TCP FIN Scan                   | sudo nmap -sF 10.10.167.5                            |
| TCP Xmas Scan                  | sudo nmap -sX 10.10.167.5                            |
| TCP Maimon Scan                | sudo nmap -sM 10.10.167.5                            |
| TCP ACK Scan                   | sudo nmap -sA 10.10.167.5                            |
| TCP Window Scan                | sudo nmap -sW 10.10.167.5                            |
| Custom TCP Scan                | sudo nmap --scanflags URGACKPSHRSTSYNFIN 10.10.167.5 |
| Spoofed Source IP              | sudo nmap -S SPOOFED_IP 10.10.167.5                  |
| Spoofed MAC Address            | --spoof-mac SPOOFED_MAC                              |
| Decoy Scan                     | nmap -D DECOY_IP,ME 10.10.167.5                      |
| Idle (Zombie) Scan             | sudo nmap -sI ZOMBIE_IP 10.10.167.5                  |
| Fragment IP data into 8 bytes  | -f                                                   |
| Fragment IP data into 16 bytes | -ff                                                  |


| **Option**             | **Purpose**                              |
| ---------------------- | ---------------------------------------- |
| --source-port PORT_NUM | specify source port number               |
| --data-length NUM      | append random data to reach given length |

These scan types rely on setting TCP flags in unexpected ways to prompt ports for a reply. Null, FIN, and Xmas scan provoke a response from closed ports, while Maimon, ACK, and Window scans provoke a response from open and closed ports.


| **Option** | **Purpose**                           |
| ---------- | ------------------------------------- |
| --reason   | explains how Nmap made its conclusion |
| -v         | verbose                               |
| -vv        | very verbose                          |
| -d         | debugging                             |
| -dd        | more details for debugging            |

---

## 📌 **`dig` Cheat Sheet** – DNS Lookup Tool

### 1. **Cú pháp cơ bản**

```bash
dig [tên_domain] [loại_record] [tùy_chọn]
```

- **`[tên_domain]`**: domain hoặc hostname cần truy vấn
    
- **`[loại_record]`**: loại bản ghi DNS (A, MX, TXT, NS…)
    
- **`[tùy_chọn]`**: các option như `+short`, `@dns_server`
    

---

### 2. **Các loại truy vấn thường gặp**

|Lệnh|Mô tả|
|---|---|
|`dig example.com`|Truy vấn bản ghi A (IPv4) mặc định|
|`dig example.com A`|Chỉ định loại record là A|
|`dig example.com AAAA`|IPv6|
|`dig example.com MX`|Mail Exchange (máy chủ email)|
|`dig example.com TXT`|Text records (SPF, DKIM, Google site verify…)|
|`dig example.com NS`|Name Server|
|`dig example.com CNAME`|Canonical Name (bí danh domain)|
|`dig example.com SOA`|Start of Authority (thông tin domain gốc)|
|`dig example.com ANY`|Tất cả các bản ghi (nhiều DNS server sẽ hạn chế trả về)|

---

### 3. **Tùy chọn hữu ích**

|Tùy chọn|Công dụng|
|---|---|
|`+short`|Hiển thị ngắn gọn kết quả|
|`+noall +answer`|Chỉ hiển thị phần trả lời|
|`+trace`|Truy vấn từng bước từ root DNS → authoritative DNS|
|`+noedns`|Tắt EDNS (hữu ích khi DNS server chặn)|
|`+dnssec`|Hiển thị chữ ký DNSSEC|
|`+multiline`|Format kết quả dễ đọc hơn|

---

### 4. **Chỉ định DNS server**

```bash
dig @8.8.8.8 example.com
```

- Dùng Google Public DNS (8.8.8.8) thay vì DNS mặc định của máy.
    

```bash
dig @1.1.1.1 example.com
```

- Dùng Cloudflare DNS.
    

---

### 5. **Truy vấn đảo ngược (Reverse Lookup)**

```bash
dig -x 8.8.8.8
```

- Tìm domain gắn với địa chỉ IP.
    

---

### 6. **Ví dụ nâng cao**

```bash
dig example.com MX +short
```

- Lấy danh sách mail server gọn nhẹ.
    

```bash
dig @8.8.8.8 example.com TXT +noall +answer
```

- Lấy record TXT từ Google DNS, chỉ hiển thị câu trả lời.
    

```bash
dig example.com ANY +short
```

- Lấy tất cả bản ghi (nếu DNS server cho phép).
    

```bash
dig example.com +trace
```

- Xem đường đi truy vấn DNS từ root tới authoritative server.
    

---

### 7. **Kết hợp với grep**

```bash
dig example.com ANY +short | grep "spf"
```

- Lọc record SPF từ kết quả.
    

---

### 8. **Ứng dụng trong pentest & OSINT**

- **Xác định hạ tầng**: MX, NS, TXT có thể tiết lộ nhà cung cấp dịch vụ hoặc IP nội bộ.
    
- **Tìm subdomain**: Kết hợp `dig` với wordlist và script.
    
- **Bypass firewall**: Dùng DNS server khác khi bị chặn.
    
- **DNS enumeration**: Thu thập thông tin tên miền phục vụ recon.
    

---
