**Di chuyển ngang (Lateral Movement)**

Di chuyển ngang là tập hợp các kỹ thuật mà kẻ tấn công sử dụng để di chuyển trong mạng. Sau khi xâm nhập vào máy đầu tiên, việc di chuyển ngang cần thiết để:
- Đạt được mục tiêu của kẻ tấn công.
- Vượt qua các hạn chế mạng.
- Tạo thêm điểm xâm nhập.
- Gây nhiễu và tránh bị phát hiện.

Quá trình này là một chu kỳ: sử dụng thông tin xác thực để di chuyển ngang, truy cập máy mới, nâng cao đặc quyền, trích xuất thông tin xác thực, và lặp lại chu kỳ.

**Ví dụ đơn giản**  
Trong một cuộc tấn công red team, mục tiêu là truy cập kho mã nội bộ. Kẻ tấn công xâm nhập ban đầu qua chiến dịch phishing, nhắm vào máy trong phòng Marketing. Máy này bị giới hạn bởi chính sách tường lửa, không thể truy cập trực tiếp kho mã. Kẻ tấn công nâng cao đặc quyền trên máy Marketing, trích xuất hash mật khẩu của quản trị viên cục bộ, và sử dụng hash này để truy cập máy DEV-001-PC của nhà phát triển. Từ đó, kho mã trở nên dễ tiếp cận.

**Di chuyển ngang đơn giản**  
Di chuyển ngang giúp vượt qua hạn chế tường lửa và tránh bị phát hiện. Kết nối từ máy của nhà phát triển tới kho mã sẽ ít bị nghi ngờ hơn so với từ máy Marketing.

**Góc nhìn của kẻ tấn công**  
Kẻ tấn công có thể sử dụng các giao thức quản trị như WinRM, RDP, VNC, hoặc SSH để di chuyển ngang, mô phỏng hành vi người dùng hợp pháp. Tuy nhiên, cần cẩn thận để tránh các kết nối đáng ngờ. Ngoài ra, kẻ tấn công có thể sử dụng các kỹ thuật nâng cao để giảm khả năng bị phát hiện.

**Quản trị viên và UAC**  
Khi thực hiện di chuyển ngang, thông tin xác thực quản trị viên thường được sử dụng, bao gồm:  
- Tài khoản cục bộ trong nhóm Administrators.  
- Tài khoản miền trong nhóm Administrators.  

User Account Control (UAC) hạn chế tài khoản quản trị viên cục bộ (trừ tài khoản Administrator mặc định), không cho phép thực hiện tác vụ quản trị từ xa qua RPC, SMB, hoặc WinRM, do đăng nhập với mã thông báo tích hợp trung bình. Tài khoản miền với đặc quyền quản trị không bị hạn chế này. UAC có thể bị vô hiệu hóa, nhưng cần lưu ý khi kỹ thuật di chuyển ngang thất bại do sử dụng tài khoản cục bộ không mặc định với UAC được bật.


"Connecting to WMI from PowerShell" mà bạn thấy trong TryHackMe thực chất là một **kỹ thuật quản trị/tấn công từ xa qua Windows Management Instrumentation (WMI)**.

---

## 1️⃣ WMI là gì?

- **Windows Management Instrumentation (WMI)** là một framework của Windows cho phép **quản lý và thu thập thông tin hệ thống** (hardware, phần mềm, process, service, event logs...) thông qua một API chuẩn.
    
- WMI thường giao tiếp qua **RPC/DCOM** (cổng TCP 135 + dynamic ports).
    
- Quản trị viên dùng WMI để cấu hình, giám sát và cài đặt phần mềm từ xa.
    

---

## 2️⃣ Kết nối WMI qua PowerShell

PowerShell có sẵn nhiều cmdlet để kết nối WMI, ví dụ:

- `Get-WmiObject` (cũ, giờ thay bằng `Get-CimInstance`)
    
- `New-CimSession` → tạo session WMI/CIM tới máy từ xa.
    
- `Invoke-CimMethod` → gọi hàm (method) trên class WMI, ví dụ cài đặt phần mềm qua `Win32_Product`.
    

Ví dụ:

```powershell
# Tạo session WMI tới máy từ xa qua DCOM
$cred = Get-Credential
$opt = New-CimSessionOption -Protocol DCOM
$session = New-CimSession -ComputerName TARGET -Credential $cred -SessionOption $opt

# Chạy method cài đặt phần mềm qua WMI
Invoke-CimMethod -CimSession $session -ClassName Win32_Product -MethodName Install -Arguments @{PackageLocation="C:\file.msi"}
```

---

## 3️⃣ Trong tấn công Lateral Movement

Trong **Red Team / Pentest**, WMI thường được dùng để:

- **Lateral Movement**: di chuyển sang máy khác trong cùng mạng bằng cách chạy lệnh từ xa.
    
- **Fileless Execution**: chạy payload mà không ghi file xuống disk.
    
- **Persistence**: tạo event subscription để trigger malware khi có sự kiện hệ thống.
    

Ví dụ tấn công:

- Attacker có credential của user trên máy B.
    
- Từ máy A, dùng `New-CimSession` → kết nối WMI tới máy B.
    
- Dùng `Invoke-CimMethod` cài payload hoặc chạy command.
    

---

## 4️⃣ Ưu điểm với attacker

- Không cần RDP hay SMB trực tiếp.
    
- Có thể chạy lệnh mà không bật màn hình victim.
    
- Khó phát hiện nếu mạng cho phép RPC/DCOM và WMI.
    

---
