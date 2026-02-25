## [[Snort]] | [[Snort ụt ụt 🐽]] | [[Protocol cho RED và BLUE]]

### Scenerio 1 | Brute-Force Attack

![[Screenshot 2025-08-17 at 15.16.49.png]]

Chạy câu lệnh `snort -dev -l .` để lấy file snort.log 
#### 🔎 Breakdown `-dev`

Ba cái này thật ra là **gộp option** lại với nhau:

- **`-d`** → Dump payload  
    Hiển thị luôn phần **payload** (dữ liệu bên trong gói tin, ví dụ HTTP request, shellcode, …).
    
- **`-e`** → Print link-layer (Ethernet header)  
    Hiển thị luôn phần header ở **tầng 2** (Ethernet) → gồm MAC nguồn, MAC đích, EtherType.
    
- **`-v`** → Verbose  
    In thêm nhiều thông tin chi tiết hơn về packet (IP src/dst, port, protocol…).

### Xác định phương thức tấn công 
![[Screenshot 2025-08-17 at 15.24.00.png]]
Sử dụng câu lệnh `sudo snort -r snort.log | grep ":22"` ta thấy được đối phương đã tấn công port 22 -> Kết luận : **SSH** 





___
### Scenerio 2 | Reverse-Shell
![[Screenshot 2025-08-17 at 15.57.13.png]]
Chạy câu lệnh `sudo snort -dev -l .` để lấy snort.log

![[Screenshot 2025-08-17 at 15.58.15.png]]
Phát hiện các port **4444** -> Kết luận của **Metasploit** 

### Ngăn chặn người Attacker dùng Metasploit
Tạo 1 rule 
`reject tcp any 4444 -> any any (msg:"Reverse Shell Detected"; sid:1000001; rev:1;)`

Sử dụng câu lệnh: `sudo snort -Q --daq afpacket -i eth0:eth1 -dev -c /etc/snort/snort.conf -A full -l .`
#### 🔎 Giải thích từng tham số

- **`sudo`**  
    Chạy với quyền root (cần để bắt gói tin trên interface).
    
- **`snort`**  
    Gọi chương trình Snort.
    
- **`-Q`**  
    Kích hoạt chế độ **inline** (chạy như IPS).  
    → Snort có thể **drop/reject** packet nếu rule yêu cầu.  
    Nếu không có `-Q`, Snort mặc định chỉ làm **IDS (chỉ alert/log, không chặn)**.
    
- **`--daq afpacket`**  
    Chỉ định **Data Acquisition module** (cơ chế mà Snort dùng để lấy packet).
    
    - `afpacket` = sử dụng Linux **AF_PACKET sockets** để đọc/gửi gói tin.
        
    - Cái này hay dùng khi chạy inline (IPS) vì nó cho phép **bridge traffic giữa 2 interface**.
        
- **`-i eth0:eth1`**  
    Chỉ định interface.
    
    - Với inline mode (`-Q`), ta thường viết `eth0:eth1` → nghĩa là bridge traffic giữa **eth0 và eth1**.
        
    - Snort sẽ đứng giữa 2 interface như một firewall trong suốt.
        
- **`-dev`**  
    = kết hợp `-d` (dump payload) + `-e` (Ethernet header) + `-v` (verbose).  
    👉 Hiển thị chi tiết packet khi match.
    
- **`-c /etc/snort/snort.conf`**  
    Load config chính của Snort (bao gồm preprocessors, variables, rule sets...).
    
- **`-A full`**  
    Chế độ alert chi tiết, in ra đầy đủ header + payload khi có rule match.
    
- **`-l .`**  
    Thư mục log = `.` → log ngay trong thư mục hiện tại.

###  Get flag thành công
![[Screenshot 2025-08-17 at 16.12.08.png]]
 **Detected Successfully**

