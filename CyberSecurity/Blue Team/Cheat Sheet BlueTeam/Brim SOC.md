## [[Brim]]

### Các Truy vấn Tùy chỉnh và Trường hợp Sử dụng trong Phân tích Lưu lượng Mạng

#### Truy vấn Brim và Mục đích

1. **Tìm kiếm cơ bản**  
   - **Cú pháp**: Tìm chuỗi hoặc giá trị số bất kỳ.  
   - **Ví dụ**: Tìm log chứa địa chỉ IP: `10.0.0.1`

2. **Toán tử logic**  
   - **Cú pháp**: OR, AND, NOT.  
   - **Ví dụ**: Tìm log chứa 3 chữ số của IP và từ khóa NTP: `192 AND NTP`

3. **Lọc giá trị**  
   - **Cú pháp**: `"tên trường" == "giá trị"`  
   - **Ví dụ**: Lọc địa chỉ IP nguồn: `id.orig_h==192.168.121.40`

4. **Liệt kê nội dung tệp log cụ thể**  
   - **Cú pháp**: `_path=="tên log"`  
   - **Ví dụ**: Liệt kê nội dung tệp log conn: `_path=="conn"`

5. **Đếm giá trị trường**  
   - **Cú pháp**: `count() by "trường"`  
   - **Ví dụ**: Đếm số tệp log có sẵn: `count() by _path`

6. **Sắp xếp kết quả**  
   - **Cú pháp**: `sort`  
   - **Ví dụ**: Đếm và sắp xếp số tệp log theo thứ tự giảm dần: `count() by _path | sort -r`

7. **Cắt trường cụ thể từ tệp log**  
   - **Cú pháp**: `_path=="conn" | cut "tên trường"`  
   - **Ví dụ**: Cắt IP nguồn, cổng đích, IP đích từ tệp conn: `_path=="conn" | cut id.orig_h, id.resp_p, id.resp_h`

8. **Liệt kê giá trị duy nhất**  
   - **Cú pháp**: `uniq`  
   - **Ví dụ**: Hiển thị các kết nối mạng duy nhất: `_path=="conn" | cut id.orig_h, id.resp_p, id.resp_h | sort | uniq`

**Lưu ý**: Nên sử dụng tên trường và bộ lọc thay vì tìm kiếm không có cấu trúc. Brim hỗ trợ lập chỉ mục tốt, nhưng hiệu suất kém với truy vấn không rõ ràng.

---

#### Các Trường hợp Sử dụng trong Phân tích An ninh Mạng

1. **Các Máy chủ Giao tiếp**  
   - **Mục đích**: Xác định danh sách máy chủ giao tiếp để phát hiện hoạt động bất thường, vi phạm truy cập, hoặc lây nhiễm mã độc.  
   - **Truy vấn**: `_path=="conn" | cut id.orig_h, id.resp_h | sort | uniq`

2. **Máy chủ Giao tiếp Thường xuyên**  
   - **Mục đích**: Phát hiện các máy chủ giao tiếp thường xuyên để nhận diện rò rỉ dữ liệu, khai thác, hoặc backdoor.  
   - **Truy vấn**: `_path=="conn" | cut id.orig_h, id.resp_h | sort | uniq -c | sort -r`

3. **Cổng Hoạt động Nhiều Nhất**  
   - **Mục đích**: Phát hiện các cổng hoạt động nhiều để tìm kiếm bất thường ẩn giấu.  
   - **Truy vấn**: 
     - `_path=="conn" | cut id.resp_p, service | sort | uniq -c | sort -r count`  
     - `_path=="conn" | cut id.orig_h, id.resp_h, id.resp_p, service | sort id.resp_p | uniq -c | sort -r`

4. **Kết nối Kéo dài**  
   - **Mục đích**: Phát hiện kết nối kéo dài bất thường, có thể là dấu hiệu của backdoor.  
   - **Truy vấn**: `_path=="conn" | cut id.orig_h, id.resp_p, id.resp_h, duration | sort -r duration`

5. **Dữ liệu Truyền tải**  
   - **Mục đích**: Tính toán kích thước dữ liệu truyền tải để phát hiện rò rỉ dữ liệu hoặc tải mã độc.  
   - **Truy vấn**: `_path=="conn" | put total_bytes := orig_bytes + resp_bytes | sort -r total_bytes | cut uid, id, orig_bytes, resp_bytes, total_bytes`

6. **Truy vấn DNS và HTTP**  
   - **Mục đích**: Phát hiện kết nối miền bất thường, kênh điều khiển và chỉ huy (C2), hoặc máy chủ bị xâm nhập.  
   - **Truy vấn**: 
     - DNS: `_path=="dns" | count() by query | sort -r`  
     - HTTP: `_path=="http" | count() by uri | sort -r`

7. **Tên Máy chủ Nghi ngờ**  
   - **Mục đích**: Phát hiện máy chủ bất thường qua thông tin tên máy và miền từ log DHCP.  
   - **Truy vấn**: `_path=="dhcp" | cut host_name, domain`

8. **Địa chỉ IP Nghi ngờ**  
   - **Mục đích**: Phát hiện địa chỉ IP bất thường từ log kết nối.  
   - **Truy vấn**: `_path=="conn" | put classnet := network_of(id.resp_h) | cut classnet | count() by classnet | sort -r`

9. **Phát hiện Tệp**  
   - **Mục đích**: Kiểm tra tệp được truyền tải để phát hiện mã độc hoặc tệp nhạy cảm.  
   - **Truy vấn**: `filename!=null`

10. **Hoạt động SMB**  
    - **Mục đích**: Phát hiện khai thác, di chuyển ngang, hoặc chia sẻ tệp độc hại qua SMB.  
    - **Truy vấn**: `_path=="dce_rpc" OR _path=="smb_mapping" OR _path=="smb_files"`

11. **Mẫu Hành vi Đã Biết**  
    - **Mục đích**: Phân tích các cảnh báo từ Zeek và Suricata để tìm dấu hiệu tấn công hoặc bất thường.  
    - **Truy vấn**: `event_type=="alert" OR _path=="notice" OR _path=="signatures"`