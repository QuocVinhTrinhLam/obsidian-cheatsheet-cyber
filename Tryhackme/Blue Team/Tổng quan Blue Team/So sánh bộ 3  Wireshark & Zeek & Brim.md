# 🔍 Comparison: Brim vs Wireshark vs Zeek

| Tool        | Mục tiêu chính | Điểm mạnh | Điểm yếu | Use Case điển hình |
|-------------|---------------|-----------|----------|---------------------|
| **Wireshark** | Packet-level analysis (deep dive từng gói tin) | - Giao diện trực quan<br>- Có thể thấy mọi thứ từ Layer 2–7<br>- Phân tích protocol cực chi tiết | - Không phù hợp để xử lý PCAP lớn (hàng GB)<br>- Khó filter khi data quá nhiều<br>- Thiếu khả năng tổng hợp thống kê | - Debug network lỗi<br>- Check protocol bất thường<br>- Xem malware giao tiếp chi tiết |
| **Zeek** | Network Security Monitoring (NSM) & Log generation | - Tự động parse PCAP thành log structured<br>- Có script để detect pattern/phân tích nâng cao<br>- Rất mạnh khi chạy **real-time** trên network | - Output nhiều file log, hơi khó đọc raw<br>- Cần học scripting (Zeek lang)<br>- Không có GUI | - Triển khai trên sensor để log toàn bộ traffic<br>- Incident response, correlation với SIEM |
| **Brim** | GUI log/PCAP analysis (Zeek + Query + Visualization) | - Kết hợp sức mạnh của Zeek + giao diện dễ dùng<br>- Timeline view rõ ràng<br>- Tích hợp mở packet bằng Wireshark<br>- Query với **Zed** để filter nhanh | - Chưa mạnh bằng SIEM full-feature<br>- Phụ thuộc vào Zeek để parse PCAP<br>- Query language (Zed) cần học | - Import PCAP & hunting<br>- SOC analyst điều tra IOC<br>- Bridge giữa raw packet (Wireshark) & log (Zeek) |

## 👉 Nói ngắn gọn:

- **Wireshark** = **Kính hiển vi** → zoom từng packet.
    
- **Zeek** = **Máy log** → biến traffic thành dữ liệu có cấu trúc.
    
- **Brim** = **Dashboard SOC mini** → combine cả hai, cho SOC analyst dễ nhìn, dễ query, dễ chia sẻ.
