anchor text

`<a href="http://spoofsite.thm">Click Here</a>`
`<a href="http://spoofsite.thm">https://onlinebank.thm</a>`

**Cơ sở hạ tầng cho chiến dịch phishing thành công**:

- **Tên miền**: Đăng ký tên miền trông đáng tin hoặc giả mạo danh tính của một tên miền khác.
- **Chứng chỉ SSL/TLS**: Tạo chứng chỉ SSL/TLS cho tên miền để tăng tính xác thực.
- **Máy chủ/ Tài khoản email**: Thiết lập máy chủ email hoặc đăng ký nhà cung cấp SMTP.
- **Bản ghi DNS**: Cấu hình SPF, DKIM, DMARC để tăng khả năng email đến hộp thư đến, tránh thư rác.
- **Máy chủ web**: Thiết lập máy chủ web hoặc mua dịch vụ lưu trữ để host website lừa đảo, thêm SSL/TLS để tăng độ tin cậy.
- **Phân tích dữ liệu**: Theo dõi email gửi, mở, nhấp chuột; kết hợp với dữ liệu từ website lừa đảo để xác định người dùng cung cấp thông tin hoặc tải phần mềm.

**Công cụ tự động hóa**:
- **GoPhish** (getgophish.com): 
  - Khung mã nguồn mở, hỗ trợ thiết lập chiến dịch phishing.
  - Lưu cài đặt SMTP, tạo mẫu email bằng trình chỉnh sửa WYSIWYG.
  - Lên lịch gửi email, cung cấp bảng phân tích (email gửi, mở, nhấp).
- **SET (Social Engineering Toolkit)** (trustedsec.com): 
  - Cung cấp công cụ tạo tấn công spear-phishing và website giả mạo để lừa người dùng nhập thông tin đăng nhập.

**Droppers**:
- Phần mềm mà nạn nhân phishing bị lừa tải xuống và chạy trên hệ thống.
- Thường giả danh phần mềm hợp pháp (như codec để xem video hoặc phần mềm mở tệp).
- Không độc hại nên thường vượt qua kiểm tra antivirus.
- Sau khi cài đặt, dropper giải nén hoặc tải mã độc từ máy chủ, cài đặt lên máy nạn nhân.
- Mã độc kết nối về cơ sở hạ tầng của kẻ tấn công, cho phép kiểm soát máy nạn nhân và khai thác mạng nội bộ.

**Tấn công phishing qua tài liệu Microsoft Office**:

- **Tài liệu Office** (Word, Excel, PowerPoint) thường được đính kèm trong chiến dịch phishing.
- **Macros**: Có thể dùng hợp pháp nhưng cũng có thể chạy lệnh cài mã độc hoặc kết nối về mạng của kẻ tấn công, cho phép kiểm soát máy nạn nhân.

**Kịch bản ví dụ**:
- Nhân viên Acme IT Support nhận email từ "nhân sự" với tệp Excel "Staff_Salaries.xlsx", giả vờ gửi nhầm thay vì gửi cho sếp.
- Thực tế: Kẻ tấn công giả mạo email nhân sự, soạn email hấp dẫn để dụ nhân viên mở tệp.
- Khi nhân viên mở tệp và bật macros, máy tính bị xâm nhập.

**Khai thác lỗ hổng trình duyệt**:

- **Khai thác trình duyệt**: Lỗ hổng trong trình duyệt (Internet Explorer/Edge, Firefox, Chrome, Safari, v.v.) cho phép kẻ tấn công chạy lệnh từ xa trên máy nạn nhân.
- **Tính khả thi**: Ít phổ biến trong đánh giá đội đỏ trừ khi biết trước về công nghệ cũ tại mục tiêu. Trình duyệt thường được cập nhật, khó khai thác, và lỗ hổng có giá trị cao nếu báo cáo cho nhà phát triển.
- **Trường hợp sử dụng**: Nhắm vào công nghệ cũ tại các tổ chức lớn (giáo dục, chính phủ, y tế) nơi phần mềm trình duyệt không thể cập nhật do tương thích với phần mềm/phần cứng thương mại.

**Kịch bản**:
- Nạn nhân nhận email dụ truy cập website do kẻ tấn công thiết lập.
- Khi truy cập, lỗ hổng trình duyệt bị khai thác, cho phép kẻ tấn công thực thi lệnh trên máy nạn nhân.

**Ví dụ**: CVE-2021-40444 (tháng 9/2021) - Lỗ hổng trong hệ thống Microsoft, cho phép thực thi mã chỉ bằng cách truy cập website.

