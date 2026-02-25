### Combined wordlists

Ghép 2 wordlists lại thành 1 wordlist bằng cách dưới đây

cewl

```shell-session
cat file1.txt file2.txt file3.txt > combined_list.txt
```

Để làm sạch wordlist được tạo mà không bị trùng từ bằng cách dưới

cewl

```shell-session
sort combined_list.txt | uniq -u > cleaned_combined_list.txt
```

### Customized Wordlist

```shell-session
user@thm$ cewl -w list.txt -d 5 -m 5 http://thm.labs
```

-w sẽ viết nội dung và file ở trên là file `list.txt`

-m 5 tập hợp các chuỗi (từ) là 5 ký tự trở lên

-d 5 là mức độ sâu của việc thu thập thông tin/nhện web (mặc định 2)

http://thm.labs is the URL that will be used


## Dictionary attack

1- What type of hash is this?  
2- What wordlist will we be using? Or what type of attack mode could we use?

To identify the type of hash, we could a tool such as hashid or hash-identifier. For this example, hash-identifier believed the possible hashing method is MD5. Please note the time to crack a hash will depend on the hardware you're using (CPU and/or GPU).

```Dictionary Attack
user@machine$ hashcat -a 0 -m 0 f806fc5a2a0d5ba2471600758452799c /usr/share/wordlists/rockyou.txt hashcat (v6.1.1) starting... f806fc5a2a0d5ba2471600758452799c:rockyou Session..........: hashcat Status...........: Cracked Hash.Name........: MD5 Hash.Target......: f806fc5a2a0d5ba2471600758452799c Time.Started.....: Mon Oct 11 08:20:50 2021 (0 secs) Time.Estimated...: Mon Oct 11 08:20:50 2021 (0 secs) Guess.Base.......: File (/usr/share/wordlists/rockyou.txt) Guess.Queue......: 1/1 (100.00%) Speed.#1.........: 114.1 kH/s (0.02ms) @ Accel:1024 Loops:1 Thr:1 Vec:8 Recovered........: 1/1 (100.00%) Digests Progress.........: 40/40 (100.00%) Rejected.........: 0/40 (0.00%) Restore.Point....: 0/40 (0.00%) Restore.Sub.#1...: Salt:0 Amplifier:0-1 Iteration:0-1 Candidates.#1....: 123456 -> 123123 Started: Mon Oct 11 08:20:49 2021 Stopped: Mon Oct 11 08:20:52 2021
```

-a 0  sets the attack mode to a dictionary attack

-m 0  sets the hash mode for cracking MD5 hashes; for other types, run hashcat -h for a list of supported hashes.

f806fc5a2a0d5ba2471600758452799c this option could be a single hash like our example or a file that contains a hash or multiple hashes.

/usr/share/wordlists/rockyou.txt the wordlist/dictionary file for our attack


## Bruteforce Attack

```Brute-force Attack
user@machine$ hashcat -a 3 -m 0 05A5CF06982BA7892ED2A6D38FE832D6 ?d?d?d?d 05a5cf06982ba7892ed2a6d38fe832d6:2021 Session..........: hashcat Status...........: Cracked Hash.Name........: MD5 Hash.Target......: 05a5cf06982ba7892ed2a6d38fe832d6 Time.Started.....: Mon Oct 11 10:54:06 2021 (0 secs) Time.Estimated...: Mon Oct 11 10:54:06 2021 (0 secs) Guess.Mask.......: ?d?d?d?d [4] Guess.Queue......: 1/1 (100.00%) Speed.#1.........: 16253.6 kH/s (0.10ms) @ Accel:1024 Loops:10 Thr:1 Vec:8 Recovered........: 1/1 (100.00%) Digests Progress.........: 10000/10000 (100.00%) Rejected.........: 0/10000 (0.00%) Restore.Point....: 0/1000 (0.00%) Restore.Sub.#1...: Salt:0 Amplifier:0-10 Iteration:0-10 Candidates.#1....: 1234 -> 6764 Started: Mon Oct 11 10:54:05 2021 Stopped: Mon Oct 11 10:54:08 2021
```

-a 3  sets the attacking mode as a brute-force attack

?d?d?d?d the ?d tells hashcat to use a digit. In our case, ?d?d?d?d for four digits starting with 0000 and ending at 9999

--stdout print the result to the terminal

Now let's apply the same concept to crack the following MD5 hash: 05A5CF06982BA7892ED2A6D38FE832D6 a four-digit PIN number.

## Rule-based Attack (Hybrid)

```Rule-based Attack
user@machine$ john --wordlist=/tmp/single-password-list.txt --rules=best64 --stdout | wc -l Using default input encoding: UTF-8 Press 'q' or Ctrl-C to abort, almost any other key for status 76p 0:00:00:00 100.00% (2021-10-11 13:42) 1266p/s pordpo 76
```

--wordlist= to specify the wordlist or dictionary file. 

--rules to specify which rule or rules to use.

--stdout to print the output to the terminal.

|wc -l  to count how many lines John produced.

### Quy tắc John trong tệp john.conf

**Thêm quy tắc vào /etc/john/john.conf:**

```plaintext
[List.Rules:THM-Password-Attacks]
Az"[0-9]" ^[!@#$]
```

- **[List.Rules:THM-Password-Attacks]**: Xác định tên quy tắc là THM-Password-Attacks.
- **Az**: Đại diện cho một từ đơn trong danh sách từ (wordlist) được chỉ định bằng -p.
- **"[0-9]"**: Thêm một chữ số (từ 0-9) vào cuối từ. Để thêm hai chữ số, sử dụng "[0-9][0-9]".
- **^[!@#$]**: Thêm một ký tự đặc biệt (!, @, #, $) vào đầu mỗi từ. Dùng $ thay ^ để thêm ký tự đặc biệt vào cuối từ.

**Tạo tệp chứa một từ "password":**

```bash
echo "password" > /tmp/single.lst
```

**Chạy John với quy tắc và hiển thị kết quả:**

```bash
john --wordlist=/tmp/single.lst --rules=THM-Password-Attacks --stdout
```

**Kết quả:**
```
!password0
@password0
#password0
$password0
```

**Thực hành**: Tạo quy tắc riêng của bạn trong john.conf để mở rộng danh sách từ.

