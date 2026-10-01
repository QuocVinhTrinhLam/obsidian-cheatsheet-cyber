**Khai thác sau khi xâm nhập (Post-Exploitation Enumeration)**

**Mục đích**: Thu thập thêm thông tin để mở rộng quyền truy cập trong mạng mục tiêu, ví dụ: tìm thông tin đăng nhập để truy cập hệ thống khác.

**Công cụ và kỹ thuật**:
- Sử dụng các công cụ có sẵn trên hệ thống (Linux: bash, Windows: cmd.exe, PowerShell) để thu thập thông tin mà không gây chú ý.
- Một số công cụ hoạt động hiệu quả ngay cả với tài khoản không có đặc quyền (non-root/admin).
- **WinPEAS** và **LinPEAS**: Script hỗ trợ kiểm tra leo thang đặc quyền trên Windows và Linux.

**Giao diện dòng lệnh**:
- Linux: Dễ dàng chuyển đổi giữa các shell (ví dụ: từ bash sang shell khác).
- Windows: Chuyển từ cmd.exe sang PowerShell bằng lệnh `powershell.exe`.

**Mục đích**: Thu thập thông tin chi tiết về hệ thống và mạng để:
- Xoay vòng (pivot) sang các hệ thống khác trong mạng.
- Thu thập dữ liệu từ hệ thống hiện tại.

**Thông tin cần thu thập**:
- **Người dùng và nhóm**: Danh sách tài khoản và nhóm trên hệ thống.
- **Tên máy chủ (Hostnames)**: Xác định danh tính hệ thống.
- **Bảng định tuyến (Routing tables)**: Thông tin về cấu hình mạng.
- **Chia sẻ mạng (Network shares)**: Các tài nguyên mạng có thể truy cập.
- **Dịch vụ mạng (Network services)**: Các dịch vụ đang chạy.
- **Ứng dụng và banner**: Thông tin về phần mềm và phiên bản.
- **Cấu hình tường lửa (Firewall configurations)**: Quy tắc bảo mật mạng.
- **Cài đặt dịch vụ và kiểm tra (Service settings and audit configurations)**: Cấu hình chi tiết của dịch vụ.
- **Chi tiết SNMP và DNS**: Thông tin quản lý mạng và tên miền.
- **Tìm kiếm thông tin đăng nhập**: Mật khẩu lưu trong trình duyệt hoặc ứng dụng.

**Dữ liệu nhạy cảm**:
- **Khóa SSH**: Cặp khóa công khai/riêng tư để truy cập hệ thống khác.
- **Tài liệu người dùng**: Tệp như `passwords.txt` hoặc `passwords.xlsx` chứa thông tin đăng nhập.
- **Mã nguồn**: Có thể chứa khóa hoặc mật khẩu nhúng, đặc biệt trong mã không công khai.

---
# Các tool để Enumeration
**Sysinternals Suite**  
- **Mô tả**: Bộ công cụ dòng lệnh và GUI cung cấp thông tin về các khía cạnh của hệ thống Windows.  
- **Ví dụ công cụ**:  
  - **Process Explorer**: Hiển thị các tiến trình, tệp đang mở và khóa registry.  
  - **Process Monitor**: Giám sát hệ thống tệp, tiến trình và registry theo thời gian thực.  
  - **PsList**: Cung cấp thông tin về các tiến trình.  
  - **PsLoggedOn**: Hiển thị người dùng đang đăng nhập.  
- **Tài nguyên**: Xem **Sysinternals Utilities Index** hoặc phòng **Sysinternals** để biết thêm chi tiết.  [](https://learn.microsoft.com/en-us/sysinternals/downloads/sysinternals-suite)

**Process Hacker**  
- **Mô tả**: Công cụ GUI cho Windows, cung cấp thông tin chi tiết về:  
  - Các tiến trình đang chạy.  
  - Kết nối mạng hoạt động.  
  - Sử dụng tài nguyên hệ thống (CPU, bộ nhớ, ổ đĩa, mạng).  

**GhostPack Seatbelt**  
- **Mô tả**: Công cụ C# trong bộ GhostPack, dùng để thu thập thông tin hệ thống.  
- **Lưu ý**: Không có bản nhị phân chính thức, cần tự biên dịch bằng MS Visual Studio.