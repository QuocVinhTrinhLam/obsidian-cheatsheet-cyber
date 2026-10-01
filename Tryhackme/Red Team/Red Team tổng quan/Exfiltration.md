## [[Lab Data Exfiltrate]] | [[Data Exfiltration from MITRE]]

**Data Exfiltration (Exfiltrate Data)** là một kỹ thuật trong tấn công mạng, thuộc giai đoạn **Action on Objectives** (theo Cyber Kill Chain) hoặc **TA0010 - Exfiltration** trong MITRE ATT&CK. Đây là quá trình **kẻ tấn công lấy dữ liệu nhạy cảm từ hệ thống mục tiêu và chuyển ra ngoài** mà không bị phát hiện.

---

### ✅ **1. Mục tiêu của Data Exfiltration**

- **Đánh cắp thông tin quan trọng**: dữ liệu cá nhân (PII), thông tin tài chính, IP (Intellectual Property), mã nguồn, tài liệu mật.
    
- **Chuẩn bị cho các cuộc tấn công tiếp theo**: như Social Engineering, Credential Stuffing, hoặc bán dữ liệu trên dark web.
    
- **Gây thiệt hại kinh doanh và uy tín** cho tổ chức.
    

---

### ✅ **2. Các Phương Pháp Exfiltration Phổ Biến**

MITRE ATT&CK định nghĩa nhiều kỹ thuật:

- **T1041 - Exfiltration Over Command and Control Channel**  
    Dữ liệu được gửi qua cùng kênh C2 (HTTP, HTTPS, DNS tunnel).
    
- **T1048 - Exfiltration Over Alternative Protocol**  
    Sử dụng FTP, SFTP, SMTP, hoặc custom protocol.
    
- **T1040 - Exfiltration Over Physical Medium**  
    Copy dữ liệu ra USB, ổ cứng ngoài.
    
- **T1567 - Exfiltration Over Web Services**  
    Upload dữ liệu lên Google Drive, Dropbox, OneDrive.
    
- **T1071 - Exfiltration via Application Layer Protocol**  
    Lợi dụng giao thức hợp pháp (HTTP/HTTPS) để che giấu.
    

---

### ✅ **3. Indicators of Data Exfiltration**

- **Tăng đột biến về lưu lượng outbound** (đặc biệt vào giờ bất thường).
    
- **Kết nối bất thường** đến dịch vụ cloud hoặc IP lạ.
    
- **Traffic mã hóa nhưng đến domain không phổ biến**.
    
- **Sử dụng các tool nghi vấn**: `rsync`, `scp`, `wget`, `curl`.
    

---

### ✅ **4. Công Cụ Thường Dùng**

- **rsync, scp, sftp**: Copy dữ liệu qua SSH.
    
- **Netcat / Ncat**: Chuyển file qua TCP/UDP.
    
- **PowerShell**: Encode + gửi dữ liệu qua HTTP.
    
- **DNS Tunneling Tools**: `iodine`, `dnscat2`.
    
- **Custom Malware**: Nén (ZIP/RAR) và mã hóa dữ liệu trước khi gửi đi.
    

---

### ✅ **5. Phòng Thủ & Giảm Thiểu**

- **DLP (Data Loss Prevention)** để giám sát và chặn dữ liệu nhạy cảm.
    
- **Firewall & IDS/IPS**: Kiểm soát outbound traffic bất thường.
    
- **Network Segmentation**: Hạn chế dữ liệu quan trọng trong vùng riêng.
    
- **Giám sát hành vi người dùng (UEBA)**: Nhận diện bất thường.
    
- **Block USB / Media**: Chính sách chống sao chép dữ liệu vật lý.
    

---

