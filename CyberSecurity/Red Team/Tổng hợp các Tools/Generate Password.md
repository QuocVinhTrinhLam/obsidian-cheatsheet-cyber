# Crunch | Cupp | Cewl

## 🧨 **1. Crunch – Tạo wordlist theo quy tắc tùy chỉnh**

Công cụ tạo wordlist offline mạnh

`crunch <min> <max> -t <pattern> -o <outputfile>`

#### Ví dụ 1: Tạo tất cả mật khẩu từ 4 đến 6 ký tự chỉ gồm số:

`crunch 4 6 0123456789 -o numbers.txt`

#### 🔹 Ví dụ 2: Tạo mật khẩu 8 ký tự theo pattern:

`crunch 8 8 -t @@%%@@%% -o pattern.txt`

- `@` = chữ thường
    
- `,` = chữ hoa
    
- `%` = số
    
- `^` = ký tự đặc biệt
    

#### 🔹 Ví dụ 3: Tạo wordlist 6 ký tự từ "abc123":

`crunch 6 6 abc123 -o abc123.txt`



## 🤖 **2. CUPP (Common User Passwords Profiler)**

CUPP tạo wordlist từ **thông tin cá nhân** (tên, sinh nhật, thú cưng...) – rất hữu ích để tấn công kiểu "social engineering".

### ✅ Cài đặt:

`git clone https://github.com/Mebus/cupp.git cd cupp python3 cupp.py`

### ✅ Cách sử dụng:

#### 🔹 Tạo wordlist bằng giao diện tương tác:

`python3 cupp.py -i`

Nó sẽ hỏi:

- Tên nạn nhân
    
- Biệt danh
    
- Ngày sinh
    
- Tên thú cưng
    
- Sở thích...
    

Sau đó xuất file `.txt` chứa danh sách mật khẩu được kết hợp từ các thông tin đó.

#### 🔹 Tạo wordlist nhanh bằng cách chỉ định:

`python3 cupp.py -w -i`

#### 🔹 Merge wordlist có sẵn:

`python3 cupp.py -l existing_list.txt`


 # 💡 So sánh nhanh:

| Công cụ    | Mục tiêu           | Đặc điểm                                         |
| ---------- | ------------------ | ------------------------------------------------ |
| **Crunch** | Brute-force        | Tạo wordlist cực lớn, theo mẫu                   |
| **CUPP**   | Social Engineering | Cá nhân hóa wordlist dựa trên thông tin nạn nhân |

## 🕵️‍♂️ **3. CEWL**

**CEWL (Custom Word List generator)** là tool viết bằng Ruby, dùng để crawl (thu thập) từ một website và trích xuất **các từ tiếng Anh (hoặc tên riêng...)** xuất hiện trên đó để tạo thành **wordlist**.

🧠 Ý tưởng: nếu bạn tấn công một công ty có website như `example.com`, thì mật khẩu của họ **có thể liên quan đến tên công ty, tên nhân viên, sản phẩm...**, và bạn có thể trích xuất các từ đó bằng `cewl`.

## 💻 ** Cách sử dụng CEWL cơ bản**

### 🔹 Crawl website và tạo wordlist:

`cewl http://example.com -w wordlist.txt`

- `-w wordlist.txt`: ghi danh sách từ vào file
    
- Mặc định, `cewl` lấy các từ có **độ dài ≥ 6**
    

### 🔹 Lấy từ có độ dài ngắn hơn (VD: ≥ 3 ký tự):

`cewl -m 3 http://example.com -w customlist.txt`

- `-m 3`: chỉ định độ dài từ tối thiểu
    

### 🔹 Crawl nhiều cấp (deep crawl):

`cewl -d 2 http://example.com -w deep.txt`

- `-d 2`: crawl 2 cấp liên kết nội bộ (internal links)
    

### 🔹 Sử dụng với user-agent hoặc đăng nhập:

`cewl -u "username:password" http://example.com -w private.txt`

---

## 🎯 **Khi nào nên dùng CEWL?**

|Tình huống|Lý do dùng CEWL|
|---|---|
|Website có nhiều tên riêng|Dễ làm wordlist tên người|
|Trang công ty, blog cá nhân|Dễ có keyword mật khẩu (tên công ty, sản phẩm, slogan...)|
|Muốn tạo wordlist "customized"|Hiệu quả hơn brute force|

---

## 🔧 **Kết hợp CEWL với John / Hydra**

Sau khi dùng `cewl`:

`john --wordlist=wordlist.txt --format=raw-md5 hashes.txt`

Hoặc:

`hydra -l admin -P wordlist.txt http://target/login`

---

## ✅ Tóm tắt nhanh

|Tool|Mục đích|
|---|---|
|`crunch`|Tạo wordlist theo pattern/ký tự|
|`cupp`|Tạo wordlist từ info cá nhân|
|`cewl`|Tạo wordlist từ **nội dung website**|