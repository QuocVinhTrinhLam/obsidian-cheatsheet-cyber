## [[So sánh bộ 3  Wireshark & Zeek & Brim]]
## **Giám sát mạng**  
Giám sát mạng là tập hợp các hành động quản lý để theo dõi liên tục và lưu trữ lưu lượng mạng (nếu cần) nhằm phát hiện, giảm thiểu vấn đề mạng, cải thiện hiệu suất và đôi khi tăng năng suất. Đây là một phần chính trong hoạt động hàng ngày của IT/SOC, khác với Giám sát An ninh Mạng (NSM) về mục đích.  

- **Mục tiêu**: Tập trung vào tài sản IT như thời gian hoạt động (uptime), sức khỏe thiết bị, chất lượng kết nối (hiệu suất), và quản lý cân bằng lưu lượng mạng (cấu hình).  
- **Chức năng**: Theo dõi, trực quan hóa lưu lượng mạng, khắc phục sự cố và phân tích nguyên nhân gốc rễ.  
- **Hạn chế**: Không tập trung vào phát hiện lỗ hổng sâu, mối đe dọa nội bộ hoặc lỗ hổng zero-day.  
- **Phạm vi**: Thường thuộc đội quản lý mạng/IT, không thuộc SOC.  

## **Giám sát An ninh Mạng (NSM)**  
- **Mục tiêu**: Tập trung vào phát hiện bất thường như máy chủ không rõ nguồn gốc, lưu lượng mã hóa, sử dụng dịch vụ/cổng đáng ngờ, và mẫu lưu lượng độc hại trong cách tiếp cận phát hiện và phản ứng xâm nhập/bất thường.  
- **Chức năng**: Theo dõi, trực quan hóa lưu lượng, điều tra sự kiện đáng ngờ, phát hiện mối đe dọa, lỗ hổng và vấn đề an ninh bằng quy tắc, chữ ký và mẫu.  
- **Phạm vi**: Thuộc SOC, phân chia công việc giữa các cấp phân tích viên (tier 1-2-3).  

# **Zeek là gì?**  
Zeek (trước đây là Bro) là công cụ giám sát mạng thụ động mã nguồn mở và thương mại, phân tích lưu lượng mạng, phát triển bởi Lawrence Berkeley Labs, hiện được hỗ trợ bởi Corelight (phiên bản thương mại).  

- **Đặc điểm**: Cung cấp hơn 50 loại log trong 7 danh mục, hỗ trợ điều tra pháp y và phân tích dữ liệu.  
- **Khác biệt**: Không giống các công cụ IDS/IPS, Zeek tập trung vào phân tích lưu lượng chi tiết hơn là chỉ phát hiện mối đe dọa cụ thể.  

### **So sánh Zeek và Snort**  

| **Công cụ**    | **Zeek**                                                                                                                                | **Snort**                                                                          |
| -------------- | --------------------------------------------------------------------------------------------------------------------------------------- | ---------------------------------------------------------------------------------- |
| **Khả năng**   | Khung NSM và IDS, tập trung phân tích mạng, phát hiện sự kiện theo chuỗi.                                                               | Hệ thống IDS/IPS, tập trung vào chữ ký để phát hiện lỗ hổng, dựa trên mẫu gói tin. |
| **Nhược điểm** | Khó sử dụng, phân tích cần thực hiện bên ngoài (thủ công hoặc tự động).                                                                 | Khó phát hiện mối đe dọa phức tạp.                                                 |
| **Ưu điểm**    | Cung cấp khả năng quan sát lưu lượng chi tiết, hỗ trợ săn tìm mối đe dọa, phát hiện mối đe dọa phức tạp, ngôn ngữ kịch bản, dễ đọc log. | Dễ viết quy tắc, hỗ trợ quy tắc Cisco, cộng đồng mạnh.                             |
| **Ứng dụng**   | Giám sát mạng, điều tra lưu lượng sâu, phát hiện xâm nhập theo chuỗi sự kiện.                                                           | Phát hiện và ngăn chặn xâm nhập, dừng các cuộc tấn công đã biết.                   |

### **Kiến trúc Zeek**  
Zeek có hai lớp chính:  
1. **Event Engine**: Xử lý gói tin, phân tích nguồn, đích, giao thức, phiên, và trích xuất tệp.  
2. **Policy Script Interpreter**: Phân tích ngữ nghĩa, liên kết sự kiện bằng kịch bản Zeek.  

### **Khung Zeek**  
Zeek cung cấp các khung mở rộng chức năng trong lớp kịch bản:  
- Logging, Notice, Input, Configuration, Intelligence, Cluster, Broker Communication, Supervisor, GeoLocation, File Analysis, Signature, Summary, NetControl, Packet Analysis, TLS Decryption.  
- **Ví dụ**: Logging Framework được sử dụng cho mọi trường hợp.  

### **Đầu ra Zeek**  
- Tạo hơn 50 log trong 7 danh mục, hỗ trợ giám sát lưu lượng, phát hiện xâm nhập, săn tìm mối đe dọa, phân tích web.  
- Log được tạo tự động khi xử lý lưu lượng hoặc tệp pcap, lưu tại thư mục làm việc hoặc đường dẫn mặc định: `/opt/zeek/logs/`.  

### **Làm việc với Zeek**  
- **Chế độ hoạt động**:  
  1. Chạy như dịch vụ (dùng ZeekControl).  
  2. Xử lý tệp pcap.  
- **Kiểm tra phiên bản**: `zeek -v`  
- **Chạy dịch vụ**: Sử dụng `ZeekControl` với quyền superuser:  
  ```bash
  sudo su
  zeekctl status/start/stop
  ```  
- **Xử lý pcap**:  
  ```bash
  zeek -C -r sample.pcap
  ls -l
  ```  
  Log được lưu tại thư mục làm việc.  

**Tham số dòng lệnh chính của Zeek**:  
- `-r`: Đọc/xử lý tệp pcap.  
- `-C`: Bỏ qua lỗi kiểm tra tổng.  
- `-v`: Thông tin phiên bản.  
- `zeekctl`: Mô-đun ZeekControl.  

**Điều tra log**: Sử dụng công cụ dòng lệnh (cat, cut, grep, sort, uniq) và công cụ bổ sung (zeek-cut).
___

# ![[Pasted image 20250818201211.png]]
## **Log của Zeek**

Zeek tạo các tệp log dựa trên dữ liệu lưu lượng mạng, bao gồm thông tin về mọi kết nối, giao thức ứng dụng và các trường dữ liệu. Zeek hỗ trợ hơn 50 loại log, được phân loại thành 7 danh mục. Log của Zeek là các tệp ASCII phân tách bằng tab, dễ đọc và xử lý nhưng đòi hỏi nỗ lực. Cần hiểu biết về mạng và giao thức để liên kết log trong quá trình điều tra, xác định trọng tâm và tìm bằng chứng cụ thể.

**Cấu trúc log**:  
- Mỗi log chứa nhiều trường dữ liệu, mỗi trường lưu một phần dữ liệu lưu lượng.  
- Liên kết được thực hiện qua giá trị duy nhất gọi là **UID** (định danh phiên).  

### **Danh mục và mô tả log của Zeek**  

| **Danh mục** | **Mô tả** | **Tệp log** |  
|-------------|-----------|-------------|  
| **Network** | Log của các giao thức mạng. | conn.log, dce_rpc.log, dhcp.log, dns.log, ftp.log, http.log, irc.log, kerberos.log, mysql.log, ntlm.log, ntp.log, radius.log, rdp.log, rfb.log, sip.log, smb_cmd.log, smb_files.log, smb_mapping.log, smtp.log, snmp.log, socks.log, ssh.log, ssl.log, syslog.log, tunnel.log |  
| **Files** | Kết quả phân tích tệp. | files.log, ocsp.log, pe.log, x509.log |  
| **NetControl** | Log điều khiển và luồng mạng. | netcontrol.log, netcontrol_drop.log, netcontrol_shunt.log, netcontrol_catch_release.log, openflow.log |  
| **Detection** | Log phát hiện và chỉ báo tiềm năng. | intel.log, notice.log, notice_alarm.log, signatures.log, traceroute.log |  
| **Network Observations** | Log quan sát luồng mạng. | known_certs.log, known_hosts.log, known_modbus.log, known_services.log, software.log |  
| **Miscellaneous** | Log bổ sung về cảnh báo bên ngoài, đầu vào và lỗi. | barnyard2.log, dpd.log, unified2.log, unknown_protocols.log, weird.log, weird_stats.log |  
| **Zeek Diagnostic** | Log chẩn đoán hệ thống, hành động và thống kê. | broker.log, capture_loss.log, cluster.log, config.log, loaded_scripts.log, packet_filter.log, print.log, prof.log, reporter.log, stats.log, stderr.log, stdout.log |  

### **Tần suất cập nhật log**  

| **Tần suất** | **Tên log** | **Mô tả** |  
|--------------|-------------|-----------|  
| **Hàng ngày** | known_hosts.log | Danh sách máy chủ hoàn thành bắt tay TCP. |  
| **Hàng ngày** | known_services.log | Danh sách dịch vụ được sử dụng bởi máy chủ. |  
| **Hàng ngày** | known_certs.log | Danh sách chứng chỉ SSL. |  
| **Hàng ngày** | software.log | Danh sách phần mềm được sử dụng trên mạng. |  
| **Theo phiên** | notice.log | Các bất thường được Zeek phát hiện. |  
| **Theo phiên** | intel.log | Lưu lượng chứa mẫu hoặc chỉ báo độc hại. |  
| **Theo phiên** | signatures.log | Danh sách các chữ ký được kích hoạt. |  

### **Hướng dẫn sử dụng log**  

| **Thông tin tổng quan** | **Dựa trên giao thức** | **Phát hiện** | **Quan sát** |  
|-------------------------|-------------------------|---------------|---------------|  
| conn.log | http.log | notice.log | known_hosts.log |  
| files.log | dns.log | signatures.log | known_services.log |  
| intel.log | ftp.log | pe.log | software.log |  
| loaded_scripts.log | ssh.log | traceroute.log | weird.log |  

**Cách sử dụng log**:  
- **Thông tin tổng quan**: Xem xét kết nối tổng thể, tệp chia sẻ, kịch bản đã tải và chỉ báo. Đây là bước đầu tiên của điều tra.  
- **Dựa trên giao thức**: Tập trung vào giao thức cụ thể khi phát hiện chỉ báo đáng ngờ hoặc cần điều tra sâu hơn.  
- **Phát hiện**: Sử dụng kịch bản hoặc chữ ký để hỗ trợ phát hiện bằng các chỉ báo bổ sung.  
- **Quan sát**: Tổng hợp máy chủ, dịch vụ, phần mềm và thống kê hoạt động bất thường để phát hiện điểm thiếu sót và kết luận điều tra.  

**Xử lý log**:  
- Log của Zeek là tệp ASCII phân tách bằng tab, dễ đọc nhưng cần kỹ năng mạng và khả năng liên kết sự kiện.  
- Công cụ sử dụng: `cat`, `cut`, `grep`, `sort`, `uniq`, và công cụ phụ trợ `zeek-cut`.  
- **zeek-cut**: Rút trích cột cụ thể từ log, sử dụng "fields" (không phải "types") ở đầu tệp log.  

**Ví dụ sử dụng zeek-cut**:  
```bash
# Xem toàn bộ log conn.log
cat conn.log
#fields ts uid id.orig_h id.orig_p id.resp_h id.resp_p proto service duration orig_bytes resp_bytes conn_state local_orig local_resp missed_bytes history orig_pkts orig_ip_bytes resp_pkts resp_ip_bytes tunnel_parents
#types time string addr port addr port enum string interval count count string bool bool count string count count count count set[string]
1488571051.943250 CTMFXm1AcIsSnq2Ric 192.168.121.2 51153 192.168.120.22 53 udp dns 0.001263 36 106 SF - -0 Dd 1 64 1 134 -
1488571038.380901 CLsSsA3HLB2N6uJwW 192.168.121.10 50080 192.168.120.10 514 udp - 0.000505 234 0 S0 - -0 D 2 290 0 0 -

# Rút trích các trường uid, proto, id.orig_h, id.orig_p, id.resp_h, id.resp_p
cat conn.log | zeek-cut uid proto id.orig_h id.orig_p id.resp_h id.resp_p
CTMFXm1AcIsSnq2Ric udp 192.168.121.2 51153 192.168.120.22 53
CLsSsA3HLB2N6uJwW udp 192.168.121.10 50080 192.168.120.10 514
```

**Lưu ý**:  
- Cần hiểu mô tả từng log và mục đích để biết dữ liệu mong đợi.  
- Loại log điều tra phụ thuộc vào loại vụ việc và giả thuyết.  
- Có thể sử dụng công cụ trực quan hóa bên ngoài như ELK hoặc Splunk để hỗ trợ phân tích.
___

# ![[Pasted image 20250818201228.png]]

## **Xử lý log Zeek bằng CLI**

**Tầm quan trọng của CLI**:  
Giao diện đồ họa (GUI) thuận tiện cho xử lý nhanh và trực quan, nhưng không ổn định và hiệu quả bằng công cụ dòng lệnh (CLI) khi xử lý khối lượng dữ liệu lớn. CLI cho phép thao tác dữ liệu linh hoạt, đặc biệt khi GUI thiếu chức năng cần thiết. Kỹ năng sử dụng CLI, Berkeley Packet Filters (BPF) và biểu thức chính quy là cần thiết để tìm kiếm, xem và trích xuất dữ liệu từ log hoặc gói tin.

### **Danh mục lệnh CLI và mục đích sử dụng**  

| **Danh mục**                       | **Lệnh và cách sử dụng** |
| ---------------------------------- | ------------------------ |
| **Cơ bản**                         |                          |
| Xem lịch sử lệnh                   | `history`                |
| Thực thi lệnh thứ 10 trong lịch sử | `!10`                    |
| Thực thi lệnh trước đó             | `!!`                     |
| **Đọc tệp**                        |                          |
| Đọc tệp `sample.txt`               | `cat sample.txt`         |
| Đọc 10 dòng đầu                    | `head sample.txt`        |
| Đọc 10 dòng cuối                   | `tail sample.txt`        |
| **Tìm & Lọc**                      |                          |
| Cắt trường đầu tiên                | `cat test.txt`           |
| Cắt cột đầu tiên                   | `cat test.txt`           |
| Lọc từ khóa cụ thể                 | `cat test.txt`           |
| Sắp xếp theo thứ tự bảng chữ cái   | `cat test.txt`           |
| Sắp xếp theo thứ tự số học         | `cat test.txt`           |
| Loại bỏ dòng trùng lặp             | `cat test.txt`           |
| Đếm số dòng                        | `cat test.txt`           |
| Hiển thị số dòng                   | `cat test.txt`           |
| **Nâng cao**                       |                          |
| In dòng 11                         | `cat test.txt`           |
| In các dòng từ 10 đến 15           | `cat test.txt`           |
| In các dòng trước dòng 11          | `cat test.txt`           |
| In dòng 11                         | `cat test.txt`           |
| **Đặc biệt**                       |                          |
| Lọc các trường cụ thể của log Zeek | `cat signatures.log      |

### **Trường hợp sử dụng**  

| **Lệnh**                        | **Mô tả**                                          |
| ------------------------------- | -------------------------------------------------- |
| `sort`                          | `uniq`                                             |
| `sort`                          | `uniq -c`                                          |
| `sort -nr`                      | Sắp xếp giá trị theo số học, đảo ngược.            |
| `rev`                           | Đảo ngược chuỗi ký tự.                             |
| `cut -f 1`                      | Cắt trường 1.                                      |
| `cut -d '.' -f 1-2`             | Chia chuỗi tại dấu chấm và giữ hai trường đầu.     |
| `grep -v 'test'`                | Hiển thị các dòng không chứa chuỗi "test".         |
| `grep -v -e 'test1' -e 'test2'` | Hiển thị các dòng không chứa "test1" hoặc "test2". |
| `file`                          | Xem thông tin tệp.                                 |
| `grep -rin Testvalue1 *`        | column -t                                          |

**Lưu ý**:  
- Các lệnh trên giúp trích xuất, lọc và phân tích log Zeek hiệu quả.  
- Kết hợp các công cụ như `zeek-cut`, `grep`, `sort`, `uniq` để tối ưu hóa quá trình điều tra log.  
- Cần thực hành để nắm vững cách sử dụng các lệnh trong việc xử lý dữ liệu mạng.
___

# ![[Pasted image 20250818201408.png]]

## **Chữ ký Zeek**

Zeek hỗ trợ chữ ký (signatures) để tạo quy tắc và liên kết sự kiện nhằm phát hiện các hoạt động đáng chú ý trên mạng. Chữ ký Zeek sử dụng khớp mẫu cấp thấp, tương tự quy tắc Snort, nhưng không phải là điểm phát hiện sự kiện chính. Zeek có ngôn ngữ kịch bản và có thể liên kết nhiều sự kiện để tìm sự kiện quan tâm. Nhiệm vụ này tập trung vào chữ ký, các nhiệm vụ sau sẽ đề cập đến kịch bản Zeek.

### **Cấu trúc chữ ký Zeek**:  
Chữ ký Zeek gồm ba phần:  
1. **Signature ID**: Tên chữ ký duy nhất.  
2. **Conditions**:  
   - **Header**: Lọc tiêu đề gói tin (địa chỉ nguồn/đích, giao thức, cổng).  
   - **Content**: Lọc nội dung gói tin (giá trị/mẫu cụ thể).  
3. **Action**:  
   - Mặc định: Tạo tệp `signatures.log` khi khớp chữ ký.  
   - Bổ sung: Kích hoạt kịch bản Zeek.  

### **Điều kiện và bộ lọc phổ biến**:  

| **Trường điều kiện** | **Bộ lọc khả dụng**                                                                                                                                                                                                                                                                                                                                       |
| -------------------- | --------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| **Header**           | `src-ip`: IP nguồn. <br> `dst-ip`: IP đích. <br> `src-port`: Cổng nguồn. <br> `dst-port`: Cổng đích. <br> `ip-proto`: Giao thức (TCP, UDP, ICMP, ICMP6, IP, IP6).                                                                                                                                                                                         |
| **Content**          | `payload`: Nội dung gói tin. <br> `http-request`: Yêu cầu HTTP đã giải mã. <br> `http-request-header`: Tiêu đề HTTP phía client. <br> `http-request-body`: Nội dung yêu cầu HTTP phía client. <br> `http-reply-header`: Tiêu đề HTTP phía server. <br> `http-reply-body`: Nội dung trả lời HTTP phía server. <br> `ftp`: Dòng lệnh đầu vào của phiên FTP. |
| **Context**          | `same-ip`: Lọc địa chỉ nguồn và đích trùng lặp.                                                                                                                                                                                                                                                                                                           |
| **Action**           | `event`: Thông báo khớp chữ ký.                                                                                                                                                                                                                                                                                                                           |
| **Toán tử so sánh**  | `==`, `!=`, `<`, `<=`, `>`, `>=`                                                                                                                                                                                                                                                                                                                          |
| **Lưu ý**            | Bộ lọc chấp nhận giá trị chuỗi, số và biểu thức chính quy (regex).                                                                                                                                                                                                                                                                                        |

**Chạy Zeek với tệp chữ ký**:  
```bash
zeek -C -r sample.pcap -s sample.sig
```  
- `.sig`: Phần mở rộng của tệp chữ ký.  
- `-C`: Bỏ qua lỗi kiểm tra tổng.  
- `-r`: Đọc tệp pcap.  
- `-s`: Sử dụng tệp chữ ký.  

**Ví dụ 1: Phát hiện mật khẩu HTTP dạng văn bản rõ**  
Tạo chữ ký để phát hiện mật khẩu dạng văn bản rõ trong lưu lượng HTTP.  
- Sử dụng regex `.*` để khớp bất kỳ ký tự nào chứa cụm “password”.  
- Khi khớp, Zeek tạo cảnh báo và ghi vào `signatures.log` và `notice.log`.  

**Phân tích log**:  
```bash
zeek -C -r http.pcap -s http-password.sig
ls
# Kết quả: clear-logs.sh  conn.log  files.log  http-password.sig  http.log  http.pcap  notice.log  packet_filter.log  signatures.log

cat notice.log | zeek-cut id.orig_h id.resp_h msg
10.10.57.178 44.228.249.3 10.10.57.178: Cleartext Password Found!
10.10.57.178 44.228.249.3 10.10.57.178: Cleartext Password Found!

cat signatures.log | zeek-cut src_addr dest_addr sig_id event_msg
10.10.57.178 http-password 10.10.57.178: Cleartext Password Found!
10.10.57.178 http-password 10.10.57.178: Cleartext Password Found!

cat signatures.log | zeek-cut sub_msg
POST /userinfo.php HTTP/1.1\x0d\x0aHost: testphp.vulnweb.com\x0d\x0aUser-Agent: Mozilla/5.0...
```  

**Ví dụ 2: Phát hiện tấn công brute-force FTP**  
Tạo chữ ký để phát hiện nỗ lực đăng nhập FTP với tài khoản “admin”.  

```bash
zeek -C -r ftp.pcap -s ftp-admin.sig
cat signatures.log | zeek-cut src_addr dst_addr event_msg sub_msg | sort -r | uniq
10.234.125.254 10.121.70.151 10.234.125.254: FTP Admin Login Attempt! USER administrator
10.234.125.254 10.121.70.151 10.234.125.254: FTP Admin Login Attempt! USER admin
```  
- Chữ ký phát hiện các nỗ lực đăng nhập với tài khoản chứa “admin”, giúp nhận diện tấn công brute-force hoặc lạm dụng tài khoản admin.  

**Tối ưu hóa chữ ký FTP**:  
- Tạo chữ ký phát hiện mọi nỗ lực brute-force FTP bằng cách theo dõi mã phản hồi “530 Login incorrect”, không giới hạn ở tài khoản “admin”.  
- Kết hợp nhiều chữ ký trong một tệp `.sig` để phát hiện các sự kiện khác nhau (ví dụ: “FTP Username Input” và “FTP Brute-force Attempt”).  

**Phân tích log với chữ ký kết hợp**:  
```bash
zeek -C -r ftp.pcap -s ftp-admin.sig
cat notice.log | zeek-cut uid id.orig_h id.resp_h msg sub | sort -r | nl | uniq | sed -n '1001,1004p'
  1001 CeMYiaHA6AkfhSnd 10.234.125.254 10.121.70.151 10.234.125.254: FTP Username Input Found! USER admin
  1002 CeMYiaHA6AkfhSnd 10.234.125.254 10.121.70.151 10.121.70.151: FTP Brute-force Attempt! 530 Login incorrect.
  1003 CeDTDZ2erDNF5w7dyf 10.234.125.254 10.121.70.151 10.234.125.254: FTP Username Input Found! USER administrator
  1004 CeDTDZ2erDNF5w7dyf 10.234.125.254 10.121.70.151 10.121.70.151: FTP Brute-force Attempt! 530 Login incorrect.
```  

**Lưu ý về chữ ký**:  
- Tạo chữ ký theo ngữ cảnh cụ thể (case signatures) cho các mối đe dọa phức tạp như zero-day hoặc tấn công nội bộ.  
- Tránh tạo quá nhiều quy tắc riêng lẻ để giảm thiểu log và tránh bỏ sót bất thường thực sự.  
- Hiểu cách hoạt động của giao thức (như FTP) và mã phản hồi (RFC) để tối ưu hóa chữ ký.  

**Quy tắc Snort trong Zeek**:  
- Trước đây, Zeek (khi còn là Bro) hỗ trợ quy tắc Snort qua kịch bản `snort2bro`.  
- Hiện tại, tài liệu chính thức của Zeek cho biết kịch bản này không còn được hỗ trợ và không có trong phân phối Zeek.  

**Lưu ý**:  
- Sử dụng kỹ năng CLI (`zeek-cut`, `grep`, `sort`, `uniq`) để trích xuất và phân tích sự kiện từ log.  
- Tham khảo tài liệu RFC để hiểu mã phản hồi giao thức khi tối ưu hóa chữ ký.
___

# ![[Pasted image 20250818204044.png]]

## **Scripts Zeek**

Zeek sở hữu ngôn ngữ kịch bản hướng sự kiện mạnh mẽ, tương đương các ngôn ngữ lập trình cấp cao, cho phép điều tra và liên kết các sự kiện được phát hiện. Để thành thạo, cần dành thời gian học ngôn ngữ kịch bản Zeek. Phần này sẽ giới thiệu cơ bản về kịch bản Zeek để giúp hiểu, chỉnh sửa và tạo kịch bản đơn giản. Kịch bản có thể được dùng để áp dụng chính sách, được gọi là **kịch bản chính sách** (policy scripts).

### **Vị trí s**cripts:  
- **Kịch bản mặc định**: Không nên chỉnh sửa, nằm tại `/opt/zeek/share/zeek/base`.  
- **Kịch bản người dùng**: Tạo hoặc chỉnh sửa, nằm tại `/opt/zeek/share/zeek/site`.  
- **Kịch bản chính sách**: Nằm tại `/opt/zeek/share/zeek/policy`.  
- **Tệp cấu hình**: `/opt/zeek/share/zeek/site/local.zeek` để tự động tải kịch bản trong chế độ giám sát trực tiếp.  
- **Phần mở rộng**: Kịch bản Zeek sử dụng đuôi `.zeek`.  
- **Lưu ý**: Không chỉnh sửa thư mục `zeek/base`. Tải kịch bản trong chế độ giám sát trực tiếp bằng lệnh `@load /script/path` hoặc `@load script-name` trong tệp `local.zeek`.  

### **Chạy Zeek với kịch bản**:  
```bash
zeek -C -r sample.pcap -s sample.sig
```  
- Zeek là **hướng sự kiện**, không phải hướng gói tin, nên cần kịch bản để xử lý sự kiện quan tâm.

### **So sánh GUI và kịch bản**:  
- **GUI (Wireshark)**: Dễ trực quan nhưng khó chuyển dữ liệu sang công cụ khác để xử lý.  
- **CLI (tcpdump, tshark)**: Dễ trích xuất và chuyển dữ liệu, nhưng việc xử lý qua nhiều pipeline không được ưu tiên.  
- **Zeek**: Tự động hóa dễ dàng, kịch bản ngắn gọn và hiệu quả hơn.  

### **Ví dụ: Trích xuất tên máy chủ DHCP**  
- **tcpdump**:  
```bash
sudo tcpdump -ntr smallFlows.pcap port 67 or port 68 -e -vv | grep 'Hostname Option' | awk -F: '{print $2}' | sort -nr | uniq | nl
     1	 "vinlap01"
     2	 "student01-PC"
```  
- **tshark**:  
```bash
tshark -V -r smallFlows.pcap -Y "udp.port==67 or udp.port==68" -T fields -e dhcp.option.hostname | nl | awk NF
     1	student01-PC
     2	vinlap01
```  
- **Zeek**:  
Kịch bản 4 dòng, trong đó dòng thứ 3 chỉ định trích xuất tên máy chủ DHCP:  
```bash
zeek -C -r smallFlows.pcap dhcp-hostname.zeek
student01-PC
vinlap01
```  
- **Ưu điểm Zeek**: Kịch bản đơn giản, dễ tạo và sử dụng, giảm thiểu pipeline phức tạp.

### **Cấu trúc kịch bản Zeek**:  
- Dòng 1, 2, 4: Cú pháp định sẵn.  
- Dòng 3: Chỉ định logic trích xuất (ví dụ: tên máy chủ DHCP).  
- Zeek sử dụng **Built-In Function (Bif)** và giao thức để trích xuất thông tin từ lưu lượng.  

### **Vị trí Bif và giao thức**:  
- `/opt/zeek/share/zeek/base/bif`  
- `/opt/zeek/share/zeek/base/bif/plugins`  
- `/opt/zeek/share/zeek/base/protocols`  
- Tham khảo kho lưu trữ Zeek để biết danh sách giao thức và Bif hỗ trợ.

### **Lưu ý**:  
- Zeek scripting là một ngôn ngữ lập trình, cần thực hành để thành thạo.  
- Học và thực hành miễn phí qua nền tảng đào tạo chính thức của Zeek.  
- Kịch bản Zeek giúp tự động hóa và liên kết sự kiện hiệu quả, phù hợp cho phân tích và điều tra mạng.
___

# ![[Pasted image 20250818205210.png]]

## **Kịch bản Zeek 101 | Viết kịch bản cơ bản**

Kịch bản Zeek bao gồm toán tử, kiểu dữ liệu, thuộc tính, khai báo, câu lệnh và chỉ thị. Dưới đây là ví dụ về sự kiện `zeek_init` (khi Zeek khởi động) và `zeek_done` (khi Zeek dừng), không yêu cầu tham số.

**Chạy kịch bản**:  
```bash
zeek -C -r sample.pcap 101.zeek
```
**Kết quả**:  
```
Started Zeek!
Stopped Zeek!
```
- Kịch bản hiển thị thông báo trên terminal và tạo log trong thư mục làm việc, tách biệt với tác vụ kịch bản.

### **Trích xuất dữ liệu gói tin**:  
Sử dụng sự kiện `new_connection` để trích xuất thông tin kết nối mà không lọc hoặc sắp xếp.  
```bash
zeek -C -r sample.pcap 102.zeek
```
**Kết quả mẫu**:  
```
[id=[orig_h=192.168.121.40, orig_p=123/udp, resp_h=212.227.54.68, resp_p=123/udp], orig=[size=48, state=1, ...], resp=[size=0, state=0, ...], start_time=1488571365.706238, ...]
```
- Kết quả cung cấp khối lượng dữ liệu lớn. Cần hiểu cấu trúc dữ liệu Zeek để lọc sự kiện quan tâm, sử dụng thẻ chính (ví dụ: `c` từ `c: connection`), giá trị `id` và tên trường (tương tự trường trong log).

### **Lọc thông tin cụ thể**:  
Kịch bản sau trích xuất địa chỉ nguồn và đích cho mỗi kết nối:  
```bash
zeek -C -r sample.pcap 103.zeek
```
**Kết quả**:  
```
###########################################################
New Connection Found! Source Host: 192.168.121.2 # 58304/udp --->
Destination Host: resp: 192.168.120.22 # 53/udp <---
###########################################################
```
- Kịch bản trích xuất sự kiện quan tâm (kết nối mới) và vẫn tạo log trong thư mục làm việc. Có thể chỉnh sửa để tối ưu hóa.

### **Kịch bản Zeek 201 | Kết hợp kịch bản và chữ ký**

Kịch bản Zeek có thể tham chiếu đến chữ ký và các kịch bản khác, hỗ trợ liên kết sự kiện hiệu quả.  
**Ví dụ**: Tạo kịch bản phát hiện lượt khớp của chữ ký `ftp-admin`:  
```bash
zeek -C -r ftp.pcap -s ftp-admin.sig 201.zeek
```
**Kết quả**:  
```
Signature hit! --> #FTP-Admin Signature hit! --> #FTP-Admin
Signature hit! --> #FTP-Admin Signature hit! --> #FTP-Admin
```
- Sử dụng sự kiện `signature_match` để kiểm tra lượt khớp chữ ký và hiển thị thông báo trên terminal.

### **Kịch bản Zeek 202 | Tải kịch bản cục bộ**

**Tải tất cả kịch bản cục bộ**:  
- Kịch bản cơ sở nằm tại `/opt/zeek/share/zeek/base`.  
- Tải tất cả kịch bản cục bộ trong `local.zeek` bằng lệnh:  
```bash
zeek -C -r ftp.pcap local
```
**Kết quả**:  
```
ls
101.zeek  103.zeek  clear-logs.sh  ftp.pcap  packet_filter.log  stats.log
102.zeek  capture_loss.log  conn.log  loaded_scripts.log  sample.pcap  weird.log
```
- Tạo thêm log như `loaded_scripts.log`, `capture_loss.log`, `notice.log`, `stats.log`.  
- Lệnh `local` tải 465 kịch bản, nhưng chỉ tạo log cho kịch bản có lượt khớp hoặc kết quả.

### **Tải kịch bản cụ thể**:  
- Tải kịch bản cụ thể bằng đường dẫn:  
```bash
zeek -C -r ftp.pcap /opt/zeek/share/zeek/policy/protocols/ftp/detect-bruteforcing.zeek
```
**Kết quả**:  
```bash
cat notice.log | zeek-cut ts note msg
1024380732.223481 FTP::Bruteforcing 10.234.125.254 had 20 failed logins on 1 FTP server in 0m1s
```
- Kịch bản mặc định cung cấp thông tin chi tiết hơn (tóm tắt kết nối, số lần đăng nhập thất bại) so với kịch bản tự tạo.  

**Lưu ý**:  
- Kịch bản Zeek cho phép tự động hóa và liên kết sự kiện hiệu quả.  
- Tham khảo tài liệu Zeek và sách trực tuyến để tìm hiểu thêm về kịch bản và khung có sẵn.  
- Cần thực hành để nắm vững cú pháp và cách sử dụng kịch bản trong phân tích mạng.