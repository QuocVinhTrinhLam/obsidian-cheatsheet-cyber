| **Yara Condition** |                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                           |
| ------------------ | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| **Keyword  <br>**  |                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                           |
| Desc               | Sử dụng desc để tóm tắt nội dung quy tắc kiểm tra. Nội dung phần này không ảnh hưởng đến quy tắc, tương tự như chú thích trong mã.                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                        |
| Meta               | Phần Meta trong quy tắc Yara dùng để chứa thông tin mô tả của tác giả. Ví dụ, sử dụng desc để tóm tắt nội dung quy tắc kiểm tra. Nội dung phần này không ảnh hưởng đến quy tắc, tương tự như chú thích trong mã.                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                          |
| Strings            | Strings<br>Chuỗi (strings) dùng để tìm kiếm văn bản hoặc mã hex trong tệp hoặc chương trình. Ví dụ, để tìm tệp chứa "Hello World!":<br>yara<br>`rule helloworld_checker {  strings:  $hello_world = "Hello World!"  condition:  $hello_world  }`<br>- Chuỗi "Hello World!" được lưu trong biến $hello_world.<br>- Điều kiện $hello_world yêu cầu tệp chứa chuỗi này để khớp.<br>- Quy tắc chỉ khớp chính xác "Hello World!", không khớp "hello world" hoặc "HELLO WORLD".<br>Để tìm nhiều biến thể chuỗi, dùng any of them:<br>yara<br>`rule helloworld_checker {  strings:  $hello_world = "Hello World!"  $hello_world_lowercase = "hello world"  $hello_world_uppercase = "HELLO WORLD"  condition:  any of them  }`<br>Quy tắc khớp nếu tệp chứa bất kỳ chuỗi nào: "Hello World!", "hello world", hoặc "HELLO WORLD". |
| Conditions         | Điều kiện sử dụng các toán tử: <=, >=, !=. Ví dụ:<br>yara<br>`rule helloworld_checker {  strings:  $hello_world = "Hello World!"  condition:  #hello_world <= 10  }`<br>- Quy tắc tìm "Hello World!" và chỉ khớp nếu chuỗi xuất hiện không quá 10 lần.<br>Kết hợp điều kiện với từ khóa and, not, or. Ví dụ, kiểm tra tệp có chuỗi "Hello World!" và kích thước dưới 10KB:<br>yara<br>`rule helloworld_checker {  strings:  $hello_world = "Hello World!"  condition:  $hello_world and filesize < 10KB  }`<br>- Quy tắc chỉ khớp nếu cả hai điều kiện đúng.<br>- Ví dụ: Quy tắc không khớp nếu tệp chứa "Hello World!" nhưng kích thước lớn hơn 10KB.<br>- Quy tắc khớp nếu tệp chứa "Hello World!" và kích thước dưới 10KB.                                                                                             |
| Weight             |                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                           |
**Tham khảo** thêm về Yara [Medium](https://medium.com/malware-buddy/security-infographics-9c4d3bd891ef#18dd) - fr0gger_ -
___
### Tích hợp với các thư viện khác

#### Cuckoo Sandbox
Cuckoo Sandbox là môi trường phân tích mã độc tự động. Mô-đun này cho phép tạo quy tắc Yara dựa trên hành vi được phát hiện từ Cuckoo. Khi môi trường này thực thi mã độc, bạn có thể tạo quy tắc dựa trên các hành vi như chuỗi thời gian chạy.

#### Python PE
Mô-đun PE của Python cho phép tạo quy tắc Yara từ các thành phần của cấu trúc Windows Portable Executable (PE). Cấu trúc PE là định dạng chuẩn cho tệp thực thi và DLL trên Windows, bao gồm các thư viện lập trình được sử dụng. Phân tích nội dung tệp PE là kỹ thuật quan trọng trong phân tích mã độc, giúp nhận diện hành vi như mã hóa hoặc lây lan mà không cần kỹ thuật đảo ngược hoặc thực thi mẫu.
___
### Công cụ Yara

#### LOKI
LOKI là công cụ quét IOC (Chỉ số thỏa hiệp) mã nguồn mở miễn phí, được phát triển bởi Florian Roth. Phát hiện dựa trên 4 phương pháp:
- Kiểm tra tên tệp IOC
- Kiểm tra quy tắc Yara
- Kiểm tra hàm băm
- Kiểm tra kết nối ngược C2
LOKI hoạt động trên Windows và Linux, tải tại [GitHub](https://github.com/Neo23x0/Loki).  
Ví dụ lệnh hiển thị menu trợ giúp:  
```bash
cmnatic@thm:~/Loki$ python3 loki.py -h
```

#### THOR
THOR Lite là công cụ quét IOC và Yara đa nền tảng (Windows, Linux, macOS) của Florian Roth. Tính năng nổi bật là điều chỉnh hiệu suất để giảm tải CPU. Tải tại [Nextron Systems](https://www.nextron-systems.com/thor/), yêu cầu đăng ký danh sách gửi thư. Phiên bản miễn phí là THOR Lite, phù hợp cho khách hàng doanh nghiệp.  
Ví dụ lệnh hiển thị menu trợ giúp:  
```bash
cmnatic@thm:~$ ./thor-lite-linux-64 -h
```

#### FENRIR
FENRIR, cũng do Florian Roth phát triển, là tập lệnh bash chạy trên mọi hệ thống hỗ trợ bash (bao gồm Windows). Khắc phục vấn đề yêu cầu phức tạp của LOKI và THOR.  
Ví dụ chạy FENRIR:  
```bash
cmnatic@thm-yara:~/tools$ ./fenrir.sh
```

#### YAYA
YAYA, phát triển bởi EFF (Electronic Frontier Foundation), ra mắt tháng 9/2020, giúp quản lý nhiều kho quy tắc Yara. Hỗ trợ nhập quy tắc chất lượng cao, thêm quy tắc tùy chỉnh, vô hiệu hóa bộ quy tắc, và quét tệp. Hiện chỉ chạy trên Linux.  
Ví dụ chạy YAYA:  
```bash
cmnatic@thm-yara:~/tools$ yaya
```

#### Yargen
**YarGen** là một công cụ **tạo rule YARA tự động**.
Nó sẽ phân tích một hoặc nhiều file mẫu (malware, script, webshell, …) → tìm các chuỗi đặc trưng (unique strings) → tạo ra YARA rule để phát hiện file tương tự trong tương lai.
Ex:

`python3 yarGen.py -m /home/cmnatic/suspicious-files/file2 --excludegood -o /home/cmnatic/suspicious-files/file2.yar` 

A brief explanation of the parameters above:

- `-m` is the path to the files you want to generate rules for
- `--excludegood` force to exclude all goodware strings (_these are strings found in legitimate software and can increase false positives_)
- `-o` location & name you want to output the Yara rule
#### **Cách YARA + yarGen hoạt động**

1. **Bạn có mẫu (sample)**: Ví dụ một webshell hoặc malware mới.
    
2. **Dùng yarGen phân tích**:
    
    - Quét file và tìm các chuỗi hiếm, ít xuất hiện trong file hợp lệ nhưng lại đặc trưng cho malware.
        
    - Loại bỏ các chuỗi "noisy" như đường dẫn hệ thống, tên file DLL phổ biến…
        
3. **yarGen xuất ra file `.yar`**:
    
    - Đây là YARA rule đã sẵn sàng để sử dụng.
        
    - Rule có thể gồm nhiều chuỗi đặc trưng, điều kiện logic, kích thước file, v.v.
        
4. **Dùng YARA scan**:
    
    - Áp rule đó để quét toàn bộ hệ thống hoặc một thư mục.
        
    - Nếu có file match → khả năng cao là biến thể của malware ban đầu.
___
