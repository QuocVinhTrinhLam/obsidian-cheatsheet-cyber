## Dịch vụ Tên NetBIOS và LLMNR

NetBIOS (Network Basic Input/Output System) và LLMNR (Link-Local Multicast Name Resolution) là các giao thức chủ yếu được sử dụng bởi Microsoft Windows để nhận dạng máy chủ. LLMNR dựa trên định dạng giao thức DNS. NetBIOS cung cấp ba dịch vụ: Name Service (NetBIOS-NS), Datagram Service (NetBIOS-DGM) và Session Service (NetBIOS-SSN). Các hoạt động này sử dụng các cổng TCP và UDP cụ thể để giao tiếp. Workgroup Windows là mạng LAN peer-to-peer, trong khi triển khai dựa trên domain là mạng client-to-server hỗ trợ nhiều máy chủ qua nhiều subnet.

Lịch sử cho thấy NetBIOS, SMB và LLMNR có nhiều lỗ hổng. Một lỗ hổng LLMNR phổ biến liên quan đến việc kẻ tấn công giả mạo nguồn xác thực cho phân giải tên, đầu độc dịch vụ LLMNR và thu thập tên người dùng cùng hash NTLMv2 của nạn nhân. Các công cụ như NBNSpoof, Metasploit và Responder có thể thực hiện các tấn công này. Pupy là công cụ quản trị từ xa mã nguồn mở dựa trên Python, đa nền tảng, phổ biến trong kiểm thử xâm nhập và tấn công.

## Khai thác SMB

Chủ đề thảo luận lịch sử lỗ hổng của SMB và nhấn mạnh khai thác EternalBlue nổi tiếng. EternalBlue bị rò rỉ bởi Shadow Brokers, được sử dụng trong ransomware như WannaCry và NotPetya. Metasploit đã port khai thác EternalBlue. Ví dụ 5-2 minh họa cách sử dụng khai thác trong Metasploit, yêu cầu cấu hình RHOST và LHOST. Khi thực thi, Metasploit khởi chạy phiên Meterpreter để kiểm soát và xâm phạm hệ thống tiếp theo. Enumeration là bước quan trọng trong kiểm thử xâm nhập; các công cụ như Nmap và Enum4linux thu thập thông tin hệ thống SMB dễ bị tổn thương để khai thác bằng Metasploit.

## Đầu độc DNS Cache (DNS Cache Poisoning)

Đầu độc DNS cache là tấn công mà kẻ đe dọa thao túng bộ nhớ cache của DNS resolver bằng cách chèn dữ liệu bị hỏng, buộc máy chủ DNS trả về địa chỉ IP sai cho nạn nhân, chuyển hướng họ đến hệ thống của kẻ tấn công. Quy trình bao gồm các bước:

1. Kẻ tấn công làm hỏng cache DNS server để giả mạo một website.
2. Sau tấn công, DNS server phân giải website thành địa chỉ IP của kẻ tấn công thay vì địa chỉ đúng.
3. Nạn nhân yêu cầu địa chỉ IP của domain từ DNS server.
4. DNS server trả lời bằng địa chỉ IP của kẻ tấn công.
5. Nạn nhân gửi yêu cầu HTTP GET đến hệ thống của kẻ tấn công, kẻ tấn công giả mạo domain.

Tấn công đầu độc DNS cache có thể kết hợp kỹ thuật social engineering để lừa nạn nhân tải malware hoặc nhập dữ liệu nhạy cảm vào form và ứng dụng giả mạo.

## Khai thác SNMP

SNMP (Simple Network Management Protocol) dùng để quản lý thiết bị mạng, mỗi thiết bị có SNMP agent kết nối với SNMP server. Quản trị viên dùng SNMP để lấy thông tin, thay đổi cấu hình và thực hiện các tác vụ khác. Có nhiều phiên bản, phổ biến là SNMPv2c và SNMPv3. SNMPv2c dùng community string làm mật khẩu, SNMPv3 an toàn hơn với username và password. Cả hai phiên bản đều dễ bị tấn công nếu dùng credential yếu hoặc mặc định.

Nmap cùng NSE scripts dùng để thu thập thông tin từ thiết bị hỗ trợ SNMP và brute-force credential yếu. Công cụ snmp-check dùng để thực hiện SNMP walk nhằm thu thập thông tin thiết bị.

## Khai thác SMTP

Máy chủ SMTP không an toàn có thể bị khai thác để gửi spam, thực hiện phishing và các tấn công dựa trên email khác. Open relay là cấu hình máy chủ email bị lạm dụng cho mục đích này. Nmap cung cấp NSE script kiểm tra open relay. Các lệnh SMTP hữu ích như HELO, EHLO, VRFY dùng để đánh giá bảo mật máy chủ email. Công cụ smtp-user-enum trong Kali Linux tự động thu thập thông tin. Tắt lệnh VRFY và EXPN trên máy chủ email hiện đại cùng firewall chặn kết nối SMTP với các lệnh này giúp tăng cường bảo mật. Các khai thác máy chủ SMTP đã biết có thể tìm bằng lệnh searchsploit.

## Khai thác FTP

Máy chủ FTP thường bị lạm dụng để đánh cắp thông tin vì giao thức FTP legacy thiếu mã hóa và kiểm tra toàn vẹn. Để tăng cường bảo mật, nên dùng FTPS hoặc SFTP (có mã hóa). Một số triển khai dùng cipher yếu như Blowfish hoặc DES; nên dùng thuật toán mạnh như AES. SFTP và FTPS dùng hashing để xác minh toàn vẹn file truyền. Thực hành tốt nhất: tắt giao thức hashing yếu như MD5 hoặc SHA-1, dùng thuật toán mạnh trong họ SHA-2.

Máy chủ FTP có thể cho phép anonymous login, dễ bị khai thác. Nên tắt anonymous login trong cấu hình server. Thực hành tốt nhất khác: dùng mật khẩu mạnh, MFA, bảo mật file/folder, mã hóa file lưu trữ, khóa tài khoản admin, cập nhật phần mềm, dùng cipher mã hóa đạt chuẩn FIPS 140-2, lưu database backend trên server riêng, yêu cầu re-authentication cho session không hoạt động.

## Tấn công Pass-the-Hash

Tấn công pass-the-hash khai thác việc lưu trữ hash mật khẩu trong file SAM (Security Accounts Manager) của Windows. Kẻ tấn công dùng hash mật khẩu thu thập từ hệ thống bị xâm phạm để đăng nhập vào hệ thống khác mà không cần biết mật khẩu thật, bỏ qua quy trình nhập và chuyển đổi mật khẩu thông thường.

## Tấn công dựa trên Kerberos và LDAP

Kerberos là giao thức xác thực dùng trong Windows và nhiều ứng dụng/hệ điều hành. Active Directory dùng LDAP làm giao thức truy cập, hỗ trợ xác thực Kerberos. Các tấn công phổ biến bao gồm golden ticket và silver ticket (thao túng ticket Kerberos dựa trên hash có sẵn), unconstrained Kerberos delegation (cho phép ứng dụng tái sử dụng credential người dùng để truy cập tài nguyên trên server khác).

## Kerberoasting

Kerberoasting là tấn công trích xuất hash credential của service account từ Active Directory để crack offline. Nó khai thác triển khai mã hóa yếu và thực hành mật khẩu không phù hợp.

## Tấn công On-Path

Tấn công on-path liên quan đến việc kẻ tấn công chặn giao tiếp giữa hai thiết bị/cá nhân để đánh cắp hoặc thao túng dữ liệu, xảy ra ở Layer 2 hoặc Layer 3. Ví dụ: ARP spoofing, MAC spoofing, thao túng Spanning Tree Protocol (STP). Để bảo mật hạ tầng, áp dụng thực hành bảo mật Layer 2 như chọn VLAN không dùng, cấu hình port switch là access port, giới hạn số MAC học trên port, kiểm soát STP, tắt CDP trên port không tin cậy, tắt tất cả port trên switch mới, dùng Root Guard, triển khai 802.1X nếu có thể, và áp dụng ACL.

## Tấn công Downgrade

Trong tấn công downgrade, kẻ tấn công buộc hệ thống dùng giao thức mã hóa hoặc thuật toán hash yếu hơn, dễ bị khai thác. Ví dụ: lỗ hổng POODLE (Padding Oracle On Downgraded Legacy Encryption) trong OpenSSL. Để ngăn chặn, thường phải loại bỏ backward compatibility.

## Tấn công Thao túng Route (Route Manipulation Attacks)

Một tấn công phổ biến là BGP hijacking. Kẻ đe dọa cấu hình hoặc xâm phạm edge router để quảng bá prefix không được ủy quyền. Điều này chuyển hướng lưu lượng nạn nhân đến kẻ tấn công nếu route độc hại cụ thể hơn hoặc ngắn hơn route hợp pháp. Kẻ tấn công đôi khi dùng prefix chưa sử dụng để tránh bị chú ý từ người dùng hoặc tổ chức hợp pháp.

## Tấn công DoS và DDoS

Tấn công DoS và DDoS nhằm làm quá tải mục tiêu bằng lượng lưu lượng lớn hoặc khai thác lỗ hổng để làm sập hệ thống. Có ba loại:

- Direct DoS Attacks: Kẻ tấn công gửi gói tin trực tiếp đến nạn nhân, làm ngập băng thông hoặc cạn kiệt tài nguyên hệ thống. Ví dụ: SYN flood.

- Reflected DoS và DDoS Attacks: Kẻ tấn công gửi gói tin spoofed đến nguồn trông như từ nạn nhân, khiến nguồn vô tình tham gia tấn công. UDP thường dùng vì không có three-way handshake.

- Amplification DDoS Attacks: Loại reflected DoS mà phản hồi lớn hơn nhiều so với gói tin ban đầu của kẻ tấn công. Ví dụ: gửi truy vấn DNS đến open resolver, nhận phản hồi lớn làm ngập máy nạn nhân.

## Bypass Network Access Control (NAC)

NAC kiểm tra các thiết bị đầu cuối (endpoints) trước khi cho phép tham gia mạng có dây hoặc không dây, thực thi các chính sách như kiểm tra phần mềm bảo mật, phiên bản hệ điều hành và tình trạng vá lỗi. Kẻ tấn công có thể bypass NAC bằng cách giả mạo (spoof) phần mềm bảo mật, phiên bản hệ điều hành và tình trạng vá lỗi. Kẻ tấn công cũng có thể bypass NAC bằng cách spoof địa chỉ MAC được ủy quyền, cho phép chúng kết nối vào mạng.

## Tấn công VLAN Hopping

VLAN hopping là phương pháp giành quyền truy cập lưu lượng trên các VLAN khác mà bình thường không thể truy cập được. Có hai phương pháp chính: switch spoofing và double tagging. Switch spoofing liên quan đến việc giả mạo một switch trunking bằng cách gửi thẻ VLAN tương ứng và các giao thức trunking. Double tagging thêm hai thẻ VLAN vào một frame; hầu hết các switch chỉ loại bỏ thẻ ngoài cùng, cho phép kẻ tấn công truy cập VLAN của nạn nhân.

## Tấn công DHCP Starvation và Rogue DHCP Servers

Tấn công DHCP starvation liên quan đến việc phát sóng hàng loạt thông điệp DHCP REQUEST giả mạo với địa chỉ MAC spoofed, làm cạn kiệt địa chỉ IP khả dụng trong phạm vi (scope) của máy chủ DHCP. Khi không còn địa chỉ IP khả dụng, kẻ tấn công có thể thiết lập máy chủ DHCP giả mạo (rogue DHCP server) để trả lời các yêu cầu DHCP mới, từ đó chặn bắt (intercept) lưu lượng từ các host trên mạng.