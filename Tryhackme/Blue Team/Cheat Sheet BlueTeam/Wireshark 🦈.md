# Lọc Gói Tin

## Bộ Lọc Gói Tin (Packet Filtering)

### Bộ Lọc Thu Thập (Capture Filters)
- **Mục đích**: Lưu một phần lưu lượng mạng cụ thể.
- **Đặc điểm**: 
  - Thiết lập trước khi thu thập, không thể thay đổi trong quá trình thu thập.
  - Dựa trên byte offset, giá trị hex, mặt nạ và toán tử logic.
- **Phạm vi**: `host`, `net`, `port`, `portrange`.
- **Hướng**: `src`, `dst`, `src or dst`, `src and dst`.
- **Giao thức**: `ether`, `wlan`, `ip`, `ip6`, `arp`, `rarp`, `tcp`, `udp`.
- **Ví dụ**: Lọc lưu lượng cổng 80: `tcp port 80`.
- **Tham khảo**: Menu "Capture --> Capture Filters".

### Bộ Lọc Hiển Thị (Display Filters)
- **Mục đích**: Giảm số lượng gói tin hiển thị, có thể thay đổi trong quá trình thu thập.
- **Đặc điểm**: 
  - Hỗ trợ 3000 giao thức, cho phép tìm kiếm chi tiết ở cấp độ giao thức.
- **Ví dụ**: Lọc lưu lượng cổng 80: `tcp.port == 80`.
- **Tham khảo**: Menu "Analyse --> Display Filters".

**Lưu ý**: Không thể dùng biểu thức bộ lọc hiển thị cho thu thập và ngược lại.

## Cú Pháp Bộ Lọc
### Bộ Lọc Thu Thập
- Dựa trên byte offset, giá trị hex và toán tử logic.
- Tham khảo chi tiết trong menu "Capture --> Capture Filters".

### Bộ Lọc Hiển Thị
- Tính năng mạnh nhất của Wireshark, hỗ trợ phân tích chi tiết giao thức.
- Tham khảo chi tiết trong menu "Analyse --> Display Filters".

## Toán Tử So Sánh
| Tiếng Anh | C-Like | Mô Tả                | Ví Dụ                     |
|-----------|--------|----------------------|---------------------------|
| eq        | ==     | Bằng                 | `ip.src == 10.10.10.100`  |
| ne        | !=     | Không bằng           | `ip.src != 10.10.10.100`  |
| gt        | >      | Lớn hơn              | `ip.ttl > 250`            |
| lt        | <      | Nhỏ hơn              | `ip.ttl < 10`             |
| ge        | >=     | Lớn hơn hoặc bằng    | `ip.ttl >= 0xFA`         |
| le        | <=     | Nhỏ hơn hoặc bằng    | `ip.ttl <= 0xA`          |

## Biểu Thức Logic

|Tiếng Anh|C-Like|Mô Tả|Ví Dụ|
|---|---|---|---|
|and|&&|Logic AND|`(ip.src == 10.10.10.100) && (ip.src == 10.10.10.111)`|
|or||||
|not|!|Logic NOT|`!(ip.src == 10.10.10.222)`|

**Lưu ý**: Không nên dùng `!=value`, thay vào đó dùng `!(value)` để đảm bảo kết quả nhất quán.

## Thanh Công Cụ Lọc (Filter Toolbar)
- **Đặc điểm**:
  - Bộ lọc sử dụng chữ thường.
  - Có tính năng tự động hoàn thành, chi tiết giao thức được biểu diễn bằng dấu chấm.
- **Màu sắc**:
  - **Xanh lá**: Bộ lọc hợp lệ.
  - **Đỏ**: Bộ lọc không hợp lệ.
  - **Vàng**: Bộ lọc cảnh báo, hoạt động nhưng không đáng tin, nên thay bằng bộ lọc hợp lệ.

# Bộ lọc gói tin
## Mô Tả Bộ Lọc

| Bộ Lọc                | Biểu Thức             | Mô Tả                                      |
|-----------------------|-----------------------|--------------------------------------------|
| `tcp.port == 80`      | Hiển thị tất cả gói tin TCP sử dụng cổng 80 |
| `udp.port == 53`      | Hiển thị tất cả gói tin UDP sử dụng cổng 53 |
| `tcp.srcport == 1234` | Hiển thị tất cả gói tin TCP xuất phát từ cổng 1234 |
| `udp.srcport == 1234` | Hiển thị tất cả gói tin UDP xuất phát từ cổng 1234 |
| `tcp.dstport == 80`   | Hiển thị tất cả gói tin TCP gửi đến cổng 80 |
| `udp.dstport == 5353` | Hiển thị tất cả gói tin UDP gửi đến cổng 5353 |

# Bộ Lọc Giao Thức Mức Ứng Dụng | HTTP và DNS

## Mô Tả Bộ Lọc

| Bộ Lọc                          | Mô Tả                                           |
| ------------------------------- | ----------------------------------------------- |
| `http`                          | Hiển thị tất cả gói tin HTTP                    |
| `dns`                           | Hiển thị tất cả gói tin DNS                     |
| `http.response.code == 200`     | Hiển thị tất cả gói tin HTTP có mã phản hồi 200 |
| `dns.flags.response == 0`       | Hiển thị tất cả yêu cầu DNS                     |
| `http.request.method == "GET"`  | Hiển thị tất cả yêu cầu HTTP GET                |
| `dns.flags.response == 1`       | Hiển thị tất cả phản hồi DNS                    |
| `http.request.method == "POST"` | Hiển thị tất cả yêu cầu HTTP POST               |
| `dns.qry.type == 1`             | Hiển thị tất cả bản ghi DNS loại A              |
___
# Bộ Lọc Nâng Cao

## Bộ Lọc: "contains"

| Bộ Lọc                | Mô Tả                                                                 |
|-----------------------|----------------------------------------------------------------------|
| **contains**          | **Loại**: Toán tử so sánh<br>**Mô tả**: Tìm kiếm một giá trị trong gói tin. Nhạy cảm với chữ hoa/thường, tương tự chức năng "Find" nhưng tập trung vào một trường cụ thể.<br>**Ví dụ**: Tìm tất cả máy chủ "Apache".<br>**Quy trình**: Liệt kê tất cả gói tin HTTP có trường "server" chứa từ khóa "Apache".<br>**Cách sử dụng**: `http.server contains "Apache"` |
## Bộ Lọc: "matches"

| Bộ Lọc      | Mô Tả                                                                                                                                                                                                                                                                                                                                                       |
| ----------- | ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| **matches** | **Loại**: Toán tử so sánh<br>**Mô tả**: Tìm kiếm một mẫu biểu thức chính quy. Không nhạy cảm với chữ hoa/thường, có thể có sai số với các truy vấn phức tạp.<br>**Ví dụ**: Tìm tất cả trang .php và .html.<br>**Quy trình**: Liệt kê tất cả gói tin HTTP có trường "host" khớp với mẫu ".php" hoặc ".html".<br>**Cách sử dụng**: `http.host matches "\.(php |
## Bộ Lọc: "in"

| Bộ Lọc                | Mô Tả                                                                 |
|-----------------------|----------------------------------------------------------------------|
| **in**                | **Loại**: Thuộc tính tập hợp<br>**Mô tả**: Tìm kiếm một giá trị hoặc trường trong phạm vi/khoảng xác định.<br>**Ví dụ**: Tìm tất cả gói tin sử dụng cổng 80, 443 hoặc 8080.<br>**Quy trình**: Liệt kê tất cả gói tin TCP có trường "port" có giá trị 80, 443 hoặc 8080.<br>**Cách sử dụng**: `tcp.port in {80 443 8080}` |
## Bộ Lọc: "upper"

| Bộ Lọc                | Mô Tả                                                                 |
|-----------------------|----------------------------------------------------------------------|
| **upper**             | **Loại**: Hàm<br>**Mô tả**: Chuyển đổi giá trị chuỗi thành chữ in hoa.<br>**Ví dụ**: Tìm tất cả máy chủ "APACHE".<br>**Quy trình**: Chuyển đổi tất cả trường "server" của gói tin HTTP thành chữ in hoa và liệt kê gói tin chứa từ khóa "APACHE".<br>**Cách sử dụng**: `upper(http.server) contains "APACHE"` |

## Bộ Lọc: "lower"

| Bộ Lọc                | Mô Tả                                                                 |
|-----------------------|----------------------------------------------------------------------|
| **lower**             | **Loại**: Hàm<br>**Mô tả**: Chuyển đổi giá trị chuỗi thành chữ thường.<br>**Ví dụ**: Tìm tất cả máy chủ "apache".<br>**Quy trình**: Chuyển đổi tất cả trường "server" của gói tin HTTP thành chữ thường và liệt kê gói tin chứa từ khóa "apache".<br>**Cách sử dụng**: `lower(http.server) contains "apache"` |

## Bộ Lọc: "string"

| Bộ Lọc                | Mô Tả                                                                 |
|-----------------------|----------------------------------------------------------------------|
| **string**            | **Loại**: Hàm<br>**Mô tả**: Chuyển đổi giá trị không phải chuỗi thành chuỗi.<br>**Ví dụ**: Tìm tất cả khung có số lẻ.<br>**Quy trình**: Chuyển đổi tất cả trường "frame number" thành giá trị chuỗi và liệt kê khung kết thúc bằng số lẻ.<br>**Cách sử dụng**: `string(frame.number) matches "[13579]$"` |
