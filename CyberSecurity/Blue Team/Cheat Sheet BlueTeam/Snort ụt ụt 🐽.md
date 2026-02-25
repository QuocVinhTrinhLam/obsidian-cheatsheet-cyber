## [[Snort]] 
___
## 📌 Snort CLI Options Cheat Sheet

# Snort CLI Options Cheat Sheet

| Option | Ý nghĩa | Use Case (Khi nào dùng) |
|--------|---------|--------------------------|
| `-i <iface>` | Chỉ định network interface (eth0, wlan0, …) | Chạy Snort ở IDS mode, sniff traffic live |
| `-r <file.pcap>` | Đọc traffic từ file pcap thay vì live | Phân tích offline, test rule trên dữ liệu sẵn có |
| `-n <num>` | Số lượng packet cần đọc rồi dừng | Debug rule nhanh, chỉ lấy một phần pcap |
| `-c <file>` | Chỉ định file rule/config (local.rules, snort.conf) | Load rule bạn viết hoặc cấu hình chuẩn |
| `-A <mode>` | Kiểu alert: `fast`, `full`, `console`, `cmg` | Test rule (`console`), log chi tiết (`full`), production (`fast`) |
| `-l <dir>` | Thư mục log output | Ghi alert/packet vào thư mục để phân tích lại |
| `-K <mode>` | Kiểu log packet: `ascii`, `pcap`, `none` | Log ra file pcap để mở bằng Wireshark |
| `-T` | Test config/rule syntax | Kiểm tra rule trước khi deploy, tránh lỗi |
| `-q` | Quiet mode, giảm log noise | Production, tránh spam log |
| `-v` | Verbose: hiển thị packet summary | Debug packet cơ bản |
| `-d` | Dump payload (nội dung packet) | Phân tích exploit/payload trong gói tin |
| `-e` | Hiển thị Ethernet header | Debug network layer chi tiết |
| `-b` | Log ở dạng tcpdump binary (pcap) | Tái phân tích lại bằng tcpdump/Wireshark |
| `-X` | Hiển thị packet payload dưới dạng hex/ascii | Tìm chuỗi trong payload khi rule chưa match |

---

## 📌 Quick Examples

# Phân tích offline pcap với rule tự viết, log chi tiết
### `snort -r sample.pcap -A full -c local.rules -l .`
# Chạy IDS live trên eth0 với rule trong local.rules
### `snort -A console -i eth0 -c local.rules -l /var/log/snort`
# Test rule trong snort.conf trước khi chạy
### `snort -T -c /etc/snort/snort.conf`
# Xem raw packet (layer 2 + payload) trong pcap
### `snort -vde -r traffic.pcap`
___

