
| Kỹ thuật                                        | Mục đích                                                                       | Ví dụ                                                                           |
| ----------------------------------------------- | ------------------------------------------------------------------------------ | ------------------------------------------------------------------------------- |
| Trinh sát (Reconnaissance)                      | Thu thập thông tin về mục tiêu                                                 | Thu thập email, [OSINT](https://en.wikipedia.org/wiki/Open-source_intelligence) |
| Vũ khí hóa (Weaponization)                      | Kết hợp mục tiêu với mã khai thác. Thường tạo thành payload có thể triển khai. | Mã khai thác với backdoor, tài liệu văn phòng độc hại                           |
| Triển khai (Delivery)                           | Làm cách nào để chuyển payload đã vũ khí hóa đến mục tiêu                      | Email, trang web, USB                                                           |
| Khai thác (Exploitation)                        | Khai thác hệ thống mục tiêu để thực thi mã                                     | MS17-010, Zero-Logon, v.v.                                                      |
| Cài đặt (Installation)                          | Cài đặt phần mềm độc hại hoặc công cụ hỗ trợ khác                              | Mimikatz, Rubeus, v.v.                                                          |
| Điều khiển & Kiểm soát (Command & Control)      | Kiểm soát tài sản bị xâm phạm từ một máy chủ điều khiển từ xa                  | Empire, Cobalt Strike, v.v.                                                     |
| Hành động theo mục tiêu (Actions on Objectives) | Thực hiện mục tiêu cuối cùng: mã hóa dữ liệu, đánh cắp dữ liệu, v.v.           | Conti, LockBit2.0, v.v.                                                         |

## Rules of Engagement

- **Scope**: 10.0.0.0/24, *.internal.corp.com
- **Out of Scope**: Production database, HR system
- **Allowed Hours**: 9am–5pm, Mon–Fri
- **Prohibited**: Social Engineering, DDoS
- **Emergency Contact**: Mr. A - +84xxx
- **Stop Word**: REDSTOP
- **Objectives**: Gain access to internal CRM, exfiltrate dummy data

Ví dụ về mô phỏng APT 41

| Bước                 | Hành vi mô phỏng                |
| -------------------- | ------------------------------- |
| Recon                | Sử dụng Shodan, Google Dorking  |
| Initial Access       | Spearphishing attachment        |
| Execution            | Dùng PowerShell để chạy payload |
| Persistence          | Tạo Scheduled Task              |
| Privilege Escalation | Dùng Mimikatz dump credential   |
| Lateral Movement     | PsExec, Remote Desktop          |
| C2                   | Cobalt Strike beacon            |
| Exfiltration         | Zip dữ liệu và gửi qua C2       |


**Áp dụng biện pháp đối phó trong OPSEC (Góc nhìn đội đỏ)**  

**Định nghĩa biện pháp đối phó (theo DoD):**  
Biện pháp đối phó được thiết kế để ngăn đối thủ phát hiện thông tin quan trọng, cung cấp cách giải thích thay thế cho thông tin hoặc chỉ số quan trọng (lừa dối), hoặc ngăn chặn hệ thống thu thập thông tin của đối thủ.  

**Ví dụ biện pháp đối phó:**  
1. **Lỗ hổng: Sử dụng cùng địa chỉ IP cho nhiều hoạt động**  
   - **Vấn đề:** Sử dụng cùng IP công khai để quét mạng (Nmap), khai thác lỗ hổng (Metasploit) và lưu trữ trang phishing. Nếu đội xanh phát hiện một hoạt động và chặn IP, các hoạt động khác sẽ bị gián đoạn.  
   - **Biện pháp đối phó:** Sử dụng các địa chỉ IP khác nhau cho từng hoạt động (quét, khai thác, phishing). Điều này đảm bảo nếu một IP bị chặn, các hoạt động khác vẫn tiếp tục mà không bị ảnh hưởng.  

2. **Lỗ hổng: Cơ sở dữ liệu không bảo mật**  
   - **Vấn đề:** Cơ sở dữ liệu lưu trữ dữ liệu từ trang phishing không được bảo mật, dễ bị bot độc hại tấn công, dẫn đến rủi ro cao do các bên thứ ba tìm kiếm mục tiêu dễ dàng.  
   - **Biện pháp đối phó:** Bảo mật cơ sở dữ liệu bằng cách:  
     - Chỉ cho phép nhân sự được ủy quyền truy cập.  
     - Áp dụng mã hóa dữ liệu, xác thực mạnh và kiểm soát truy cập nghiêm ngặt.  
     - Thường xuyên cập nhật và vá lỗi hệ thống để ngăn chặn khai thác.  

**Kết luận:**  
Biện pháp đối phó trong OPSEC tập trung vào việc giảm thiểu rủi ro từ các lỗ hổng đã xác định, đảm bảo thông tin quan trọng được bảo vệ và hoạt động của đội đỏ không bị đội xanh hoặc bên thứ ba độc hại phát hiện hoặc ngăn chặn.