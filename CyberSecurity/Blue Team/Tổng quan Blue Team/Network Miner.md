## **Tổng Quan**

**Khả năng** | **Mô tả**  
---|---  
**Ngửi lưu lượng (Traffic sniffing)** | Có thể chặn, ngửi và ghi lại các gói tin đi qua mạng.  
**Phân tích tệp PCAP** | Phân tích tệp PCAP và hiển thị chi tiết nội dung gói tin.  
**Phân tích giao thức** | Xác định các giao thức được sử dụng từ tệp PCAP.  
**Nhận diện hệ điều hành (OS fingerprinting)** | Xác định hệ điều hành từ tệp PCAP, phụ thuộc vào Satori và p0f.  
**Trích xuất tệp** | Trích xuất hình ảnh, tệp HTML và email từ tệp PCAP.  
**Lấy thông tin xác thực (Credential grabbing)** | Trích xuất thông tin xác thực từ tệp PCAP.  
**Phân tích từ khóa dạng văn bản rõ (Clear text keyword parsing)** | Trích xuất từ khóa và chuỗi văn bản rõ từ tệp PCAP.  

**Chế độ hoạt động**  
1. **Chế độ ngửi (Sniffer Mode)**:  
   - Có tính năng ngửi nhưng không được thiết kế để sử dụng chính như một công cụ ngửi.  
   - Chỉ hỗ trợ ngửi trên Windows, các tính năng khác hoạt động trên cả Windows và Linux.  
   - Tính năng ngửi không đáng tin cậy, không khuyến khích sử dụng làm công cụ ngửi chính.  
   - NetworkMiner là công cụ phân tích pháp y mạng (Network Forensic Analysis Tool), không phải sniffer chuyên dụng như Wireshark hay tcpdump.  

2. **Chế độ phân tích/xử lý gói tin (Packet Parsing/Processing)**:  
   - Phân tích tệp PCAP để có cái nhìn tổng quan nhanh.  
   - Phù hợp để thu thập thông tin cơ bản trước khi phân tích sâu hơn.  

**Ưu và Nhược điểm**  

**Ưu điểm** | **Nhược điểm**  
---|---  
Nhận diện hệ điều hành | Không hiệu quả trong ngửi lưu lượng trực tiếp  
Trích xuất tệp dễ dàng | Không phù hợp cho phân tích tệp PCAP lớn  
Lấy thông tin xác thực | Tùy chọn lọc hạn chế  
Phân tích từ khóa văn bản rõ | Không thiết kế cho phân tích lưu lượng thủ công  
Tổng quan nhanh |  

## **So sánh NetworkMiner và Wireshark**  

**Tính năng** | **NetworkMiner** | **Wireshark**  
---|---|---  
**Mục đích** | Tổng quan nhanh, lập bản đồ lưu lượng, trích xuất dữ liệu | Phân tích chi tiết  
**Giao diện đồ họa** | ✅ | ✅  
**Ngửi lưu lượng** | ✅ | ✅  
**Xử lý PCAP** | ✅ | ✅  
**Nhận diện hệ điều hành** | ✅ | ❌  
**Khám phá tham số/từ khóa** | ✅ | Thủ công  
**Khám phá thông tin xác thực** | ✅ | ✅  
**Trích xuất tệp** | ✅ | ✅  
**Tùy chọn lọc** | Hạn chế | ✅  
**Giải mã gói tin** | Hạn chế | ✅  
**Phân tích giao thức** | ❌ | ✅  
**Phân tích tải trọng (Payload)** | ❌ | ✅  
**Phân tích thống kê** | ❌ | ✅  
**Hỗ trợ đa nền tảng** | ✅ | ✅  
**Phân loại máy chủ** | ✅ | ❌  
**Dễ quản lý** | ✅ | ✅  

**Thực hành tốt nhất**: Ghi lại lưu lượng để phân tích offline, sử dụng NetworkMiner để xem tổng quan PCAP và Wireshark để phân tích sâu hơn.