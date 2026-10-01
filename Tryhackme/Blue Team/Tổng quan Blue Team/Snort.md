## [[Snort ụt ụt 🐽]]  
___

## **Snort là gì?**

Snort là một **Network Intrusion Detection System (NIDS)** mã nguồn mở, phát triển bởi **Cisco**, dùng để **phát hiện và ngăn chặn** các hoạt động bất thường trong lưu lượng mạng.  
Nó có thể chạy ở **3 chế độ**:

1. **Sniffer Mode** 🕵️ — chỉ đơn giản là đọc và hiển thị gói tin ra màn hình.
    
2. **Packet Logger Mode** 🗃 — lưu toàn bộ gói tin vào file log để phân tích sau.
    
3. **NIDS Mode** 🚨 — phát hiện tấn công theo các **rules** và đưa ra cảnh báo (hoặc chặn luôn nếu chạy kiểu IPS).
    

---

## **Kiến trúc Snort**

Think of Snort như pipeline:

1. **Packet Decoder** – đọc dữ liệu từ network interface.
    
2. **Preprocessors** – làm sạch, chuẩn hóa, detect các pattern bất thường trước khi vào rule.
    
3. **Detection Engine** – so khớp với bộ rules.
    
4. **Logging & Alerting** – log hoặc bắn cảnh báo.
    
5. **Output Modules** – gửi dữ liệu tới syslog, database, SIEM…
    

---

## **Rule Snort**

Một rule Snort thường có cấu trúc:

```
action proto src_ip src_port -> dst_ip dst_port (options)
```

Ví dụ rule cảnh báo khi có HTTP traffic:

```snort
alert tcp any any -> any 80 (msg:"HTTP Traffic Detected"; sid:1000001; rev:1;)
```

- **action**: alert, log, drop…
    
- **proto**: tcp, udp, icmp…
    
- **msg**: tin nhắn cảnh báo
    
- **sid**: Snort ID của rule
    
- **rev**: version của rule
    

---

## **Các use case điển hình**

- **Phát hiện tấn công DDoS**
    
- **Phát hiện exploit** (SQL Injection, Buffer Overflow…)
    
- **Theo dõi malware C2 traffic**
    
- **Chặn kết nối tới IP xấu**
    
- **Giám sát lưu lượng nội bộ** để phát hiện data exfiltration
    
---

## **Quick start trên Kali**

```bash
sudo apt install snort
sudo snort -i eth0 -A console -c /etc/snort/snort.conf
```

- `-i eth0` : chọn interface
    
- `-A console` : in cảnh báo ra màn hình
    
- `-c` : file config
    

---

Nếu bạn muốn, mình có thể làm cho bạn **một bảng cheat sheet Snort** với syntax rules, các lệnh hay, và workflow điều tra luôn, để cắm vào Obsidian là dùng được.  
Bạn có muốn mình làm luôn không?
___

## **Sniffer Mode**

| **Parameter** | **Description**                                                                                                                                               |
| ------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| **-v**        | Verbose. Display the TCP/IP output in the console.                                                                                                            |
| **-d**        | Display the packet data (payload).                                                                                                                            |
| **-e**        | Display the link-layer (TCP/IP/UDP/ICMP) headers.                                                                                                             |
| -**X**        | Display the full packet details in HEX.                                                                                                                       |
| -**i**        | This parameter helps to define a specific network interface to listen/sniff. Once you have multiple interfaces, you can choose a specific interface to sniff. |
Start the Snort instance in **verbose mode (-v)** and **use the interface (-i)** "eth0"; `sudo snort -v -i eth0`

### **1. `snort -v`**

- **Verbose mode** → Hiển thị **IP header** của mỗi gói tin.
    
- Chỉ thấy thông tin cơ bản như:
    
    ```
    SRC IP → DST IP
    Protocol
    ```
    
- Dùng khi bạn chỉ muốn xem nhanh traffic đi đâu.
    

---

### **2. `snort -vd`**

- `-v` (IP header) + `-d` (Data payload).
    
- Hiển thị **IP header + phần data** của gói tin dưới dạng ASCII.
    
- Dùng khi muốn xem nội dung data (vd: HTTP GET request).
    

---

### **3. `snort -de`**

- `-d` (Data) + `-e` (Ethernet header).
    
- Hiển thị **Ethernet header + Data payload** (không có IP header riêng biệt như `-v`).
    
- Hữu ích khi bạn muốn xem địa chỉ **MAC** + nội dung data.
    

---

### **4. `snort -v -d -e`**

- Kết hợp cả 3:
    
    - **Ethernet header** (MAC source/dest)
        
    - **IP header**
        
    - **Data payload**
        
- Đây là dạng “full thông tin” của gói tin ở mức L2 + L3 + L7.
    
- Dùng khi cần phân tích chi tiết tất cả layer trong gói tin.
    

---

### **5. `snort -X`**

- Dump **payload** ra **cả ASCII lẫn Hex**.
    
- Giúp xem nội dung khi data không phải toàn chữ (vd: binary file, malware, shellcode).
    
- Output kiểu này hay dùng khi muốn **forensics** hoặc reverse payload exploit.
    

---

💡 **Ví dụ:**

- Bạn sniff traffic HTTP test rule SQL injection:
    
    - `snort -v` → thấy source IP + dest IP + protocol TCP
        
    - `snort -vd` → thấy luôn “GET /index.php?id=1’ OR ‘1’=‘1”
        
    - `snort -X` → thấy cả chuỗi trên + hex code của từng byte.
        

---
## **Packet Logger Mode**

| **Parameter** | **Description**                                                                                                                                                                  |
| ------------- | -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| -l            | Logger mode, target **log and alert** output directory. Default output folder is **/var/log/snort**<br><br>The default action is to dump as tcpdump format in **/var/log/snort** |
| **-K ASCII**  | Log packets in ASCII format.                                                                                                                                                     |
| -r            | Reading option, read the dumped logs in Snort.                                                                                                                                   |
| **-n**        | Specify the number of packets that will process/read. Snort will stop after reading the specified number of packets.                                                             |

## **IDS/IPS Mode**

NIDS mode parameters are explained in the table below;

| **Parameter** | **Description**                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                           |
| ------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| -c            | Defining the configuration file.                                                                                                                                                                                                                                                                                                                                                                                                                                                                                          |
| -T            | Testing the configuration file.                                                                                                                                                                                                                                                                                                                                                                                                                                                                                           |
| **-N**        | Disable logging.                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                          |
| **-D**        | Background mode.                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                          |
| **-A**        | Alert modes;  <br><br>**full:** Full alert mode, providing all possible information about the alert. This one also is the default mode; once you use -A and don't specify any mode, snort uses this mode.<br><br>**fast:**  Fast mode shows the alert message, timestamp, source and destination IP, along with port numbers.<br><br>console: Provides fast style alerts on the console screen.<br><br>**cmg:** CMG style, basic header details with payload in hex and text format.<br><br>**none:** Disabling alerting. |
___
## **Rule Snort**

![[Pasted image 20250816131519.png]]
### **General Rule Options**

| Msg       | The message field is a basic prompt and quick identifier of the rule. Once the rule is triggered, the message filed will appear in the console or log. Usually, the message part is a one-liner that summarises the event.                                                                                                                                                                                                                                                                                                                                |
| --------- | --------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| Sid       | Snort rule IDs (SID) come with a pre-defined scope, and each rule must have a SID in a proper format. There are three different scopes for SIDs shown below.<br><br>- <100: Reserved rules<br>- 100-999,999: Rules came with the build.<br>- >=1,000,000: Rules created by user.<br><br>Briefly, the rules we will create should have sid greater than 100.000.000. Another important point is; SIDs should not overlap, and each id must be unique.                                                                                                      |
| Reference | Each rule can have additional information or reference to explain the purpose of the rule or threat pattern. That could be a Common Vulnerabilities and Exposures (CVE) id or external information. Having references for the rules will always help analysts during the alert and incident investigation.                                                                                                                                                                                                                                                |
| Rev       | Snort rules can be modified and updated for performance and efficiency issues. Rev option help analysts to have the revision information of each rule. Therefore, it will be easy to understand rule improvements. Each rule has its unique rev number, and there is no auto-backup feature on the rule history. Analysts should keep the rule history themselves. Rev option is only an indicator of how many times the rule had revisions.<br><br>alert icmp any any <> any any (msg: "ICMP Packet Found"; sid: 100001; reference:cve,CVE-XXXX; rev:1;) |

### Payload Detection Rule Options

| Content      | Payload data. It matches specific payload data by ASCII, HEX or both. It is possible to use this option multiple times in a single rule. However, the more you create specific pattern match features, the more it takes time to investigate a packet.<br><br>Following rules will create an alert for each HTTP packet containing the keyword "GET". This rule option is case sensitive!<br><br>- ASCII mode - alert tcp any any <> any 80  (msg: "GET Request Found"; content:"GET"; sid: 100001; rev:1;)<br>- HEX mode - alert tcp any any <> any 80  (msg: "GET Request Found"; content:"\|47 45 54\|"; sid: 100001; rev:1;)                                                                                                           |
| ------------ | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ |
| Nocase       | Disabling case sensitivity. Used for enhancing the content searches.<br><br>alert tcp any any <> any 80  (msg: "GET Request Found"; content:"GET"; nocase; sid: 100001; rev:1;)                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                            |
| Fast_pattern | Prioritise content search to speed up the payload search operation. By default, Snort uses the biggest content and evaluates it against the rules. "fast_pattern" option helps you select the initial packet match with the specific value for further investigation. This option always works case insensitive and can be used once per rule. Note that this option is required when using multiple "content" options. <br><br>The following rule has two content options, and the fast_pattern option tells to snort to use the first content option (in this case, "GET") for the initial packet match.<br><br>alert tcp any any <> any 80  (msg: "GET Request Found"; content:"GET"; fast_pattern; content:"www";  sid:100001; rev:1;) |

### Non-Payload Detection Rule Options

There are rule options that focus on non-payload data. These options will help create specific patterns and identify network issues.

| ID     | Filtering the IP id field.<br><br>alert tcp any any <> any any (msg: "ID TEST"; id:123456; sid: 100001; rev:1;)                                                                                  |
| ------ | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ |
| Flags  | Filtering the TCP flags.<br><br>- F - FIN<br>- S - SYN<br>- R - RST<br>- P - PSH<br>- A - ACK<br>- U - URG<br><br>alert tcp any any <> any any (msg: "FLAG TEST"; flags:S;  sid: 100001; rev:1;) |
| Dsize  | Filtering the packet payload size.<br><br>- dsize:min<>max;<br>- dsize:>100<br>- dsize:<100<br><br>alert ip any any <> any any (msg: "SEQ TEST"; dsize:100<>300;  sid: 100001; rev:1;)           |
| Sameip | Filtering the source and destination IP addresses for duplication.<br><br>alert ip any any <> any any (msg: "SAME-IP TEST";  sameip; sid: 100001; rev:1;)                                        |
___
# THM Snort CheatSheet

![[SnortCheatsheet-TryHackMe-1647343678716.pdf]]