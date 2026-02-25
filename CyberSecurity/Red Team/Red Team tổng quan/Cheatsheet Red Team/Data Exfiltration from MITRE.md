# MITRE ATT&CK – Exfiltration Techniques Cheat Sheet
## [[Exfiltration]] | [[Lab Data Exfiltrate]]

| ID       | Technique                               | Mô tả ngắn                                                   | Ví dụ / Công cụ         |
|----------|----------------------------------------|--------------------------------------------------------------|-------------------------|
| **T1041**| Exfiltration Over C2 Channel          | Dữ liệu gửi qua cùng kênh C2 (HTTP/HTTPS, DNS)             | Malware custom, C2 Framework |
| **T1048**| Exfiltration Over Alternative Protocol| Sử dụng FTP, SFTP, SMTP hoặc custom protocol để gửi dữ liệu| `ftp`, `scp`, `curl`   |
| **T1567**| Exfiltration Over Web Services        | Upload dữ liệu lên dịch vụ cloud (Google Drive, Dropbox)   | API upload scripts     |
| **T1071**| Application Layer Protocol            | Dùng HTTP/HTTPS, SMTP, WebSocket để che giấu dữ liệu       | `curl`, `wget`, custom HTTP |
| **T1020**| Automated Exfiltration                | Script hoặc tool tự động gửi dữ liệu theo thời gian định kỳ| Python scripts, Cron jobs |
| **T1011**| Exfiltration Over Other Network Medium| Dùng Bluetooth, Wi-Fi Direct, mạng ad-hoc                 | Bluetooth exfil tools  |
| **T1005**| Data from Local System                | Thu thập dữ liệu từ endpoint trước khi gửi đi              | `tar`, `zip`, PowerShell |
| **T1052**| Exfiltration Over Physical Medium     | Copy dữ liệu ra USB, ổ cứng ngoài                          | `dd`, manual copy      |
| **T1537**| Transfer Data to Cloud Account        | Upload trực tiếp vào tài khoản cloud attacker kiểm soát    | Google Drive API, AWS CLI|
| **T1029**| Scheduled Transfer                    | Lập lịch gửi dữ liệu định kỳ để tránh bị phát hiện         | `cron`, Task Scheduler |

---

## 🔍 Dấu hiệu phát hiện (Detection Ideas)
- **Traffic outbound bất thường** (ngoài giờ làm việc, spike lớn)
- **Sử dụng dịch vụ cloud lạ hoặc domain không phổ biến**
- **Traffic mã hóa tới IP chưa biết**
- **Tăng tần suất DNS query bất thường**

---

## ✅ Phòng thủ (Mitigations)
- **DLP (Data Loss Prevention)** – chặn dữ liệu nhạy cảm rời mạng
- **Firewall/Proxy** – kiểm soát kết nối outbound
- **IDS/IPS** – phát hiện mẫu lưu lượng bất thường
- **Network Segmentation** – cô lập dữ liệu nhạy cảm
- **Disable USB / External Media** – hạn chế exfil vật lý

