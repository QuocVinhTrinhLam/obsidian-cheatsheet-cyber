****
**MISP (Malware Information Sharing Platform)**  
- Là nền tảng mã nguồn mở chia sẻ thông tin mối đe dọa, hỗ trợ thu thập, lưu trữ và phân phối tình báo mối đe dọa cùng các Chỉ số Thỏa hiệp (IOCs) liên quan đến phần mềm độc hại, tấn công mạng, gian lận tài chính hoặc thông tin tình báo trong cộng đồng đáng tin cậy.  
- Sử dụng mô hình phân tán, hỗ trợ cộng đồng đóng, bán riêng tư và mở (công khai).  
- Thông tin mối đe dọa có thể được phân phối và sử dụng bởi các Hệ thống Phát hiện Xâm nhập Mạng (NIDS), công cụ phân tích nhật ký và Hệ thống Quản lý Thông tin & Sự kiện Bảo mật (SIEM).

**Trường hợp sử dụng**  
- Phân tích ngược phần mềm độc hại: Chia sẻ các chỉ số để hiểu cách các họ phần mềm độc hại hoạt động.  
- Điều tra bảo mật: Tìm kiếm, xác thực và sử dụng chỉ số trong điều tra vi phạm bảo mật.  
- Phân tích tình báo: Thu thập thông tin về nhóm đối thủ và khả năng của họ.  
- Thi hành pháp luật: Sử dụng chỉ số để hỗ trợ điều tra pháp y.  
- Phân tích rủi ro: Nghiên cứu các mối đe dọa mới, khả năng xảy ra và tần suất.  
- Phân tích gian lận: Chia sẻ chỉ số tài chính để phát hiện gian lận tài chính.

**Chức năng cốt lõi**  
- **Cơ sở dữ liệu IOC**: Lưu trữ thông tin kỹ thuật và phi kỹ thuật về mẫu phần mềm độc hại, sự cố, kẻ tấn công và tình báo.  
- **Tương quan tự động**: Xác định mối quan hệ giữa các thuộc tính và chỉ số từ phần mềm độc hại, chiến dịch tấn công hoặc phân tích.  
- **Chia sẻ dữ liệu**: Cho phép chia sẻ thông tin qua các mô hình phân phối và giữa các phiên bản MISP khác nhau.  
- **Tính năng nhập/xuất**: Hỗ trợ nhập và xuất sự kiện ở các định dạng khác nhau để tích hợp với NIDS, HIDS và OpenIOC.  
- **Sơ đồ sự kiện**: Hiển thị mối quan hệ giữa các đối tượng và thuộc tính từ sự kiện.  
- **Hỗ trợ API**: Tích hợp với hệ thống riêng để lấy và xuất sự kiện, tình báo.

**Thuật ngữ thường dùng**  
- **Events**: Bộ sưu tập thông tin liên kết theo ngữ cảnh.  
- **Attributes**: Điểm dữ liệu cá nhân liên quan đến sự kiện, như chỉ số mạng hoặc hệ thống.  
- **Objects**: Tổ hợp thuộc tính tùy chỉnh.  
- **Object References**: Mối quan hệ giữa các đối tượng khác nhau.  
- **Sightings**: Sự xuất hiện cụ thể theo thời gian của một điểm dữ liệu hoặc thuộc tính để tăng độ tin cậy.  
- **Tags**: Nhãn gắn với sự kiện/thuộc tính.  
- **Taxonomies**: Thư viện phân loại dùng để gắn nhãn, phân loại và tổ chức thông tin.  
- **Galaxies**: Mục kiến thức cơ bản dùng để gắn nhãn sự kiện/thuộc tính.  
- **Indicators**: Thông tin phát hiện hoạt động mạng đáng ngờ hoặc độc hại.