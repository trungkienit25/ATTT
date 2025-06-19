### 2. Mã xác thực thông điệp (Message Authentication Code - MAC)

Mã xác thực thông điệp (MAC) là một kỹ thuật mật mã được sử dụng để đảm bảo **tính toàn vẹn** và **tính xác thực** của một thông điệp.

#### Khái niệm

* **Giả định:** Hai bên (Alice và Bob) đã chia sẻ an toàn một khóa bí mật `k` từ trước. 
* Một hàm MAC bao gồm một cặp thuật toán `(S, V)`: 
    * **Hàm sinh mã (S - Sign):** Alice sử dụng khóa bí mật `k` để tính một giá trị xác thực, gọi là thẻ (tag) `t`, cho một thông điệp `m`. 
        `t = S(k, m)`
        Thẻ `t` có kích thước cố định, không phụ thuộc vào kích thước của thông điệp `m`. 
    * **Hàm xác minh (V - Verify):** Alice gửi cả `m` và `t` cho Bob. Bob, người cũng biết khóa `k`, sẽ thực hiện xác minh. 
        `V(k, m, t)`
        Bob tự tính lại thẻ `t* = S(k, m)` và so sánh với thẻ `t` nhận được. Nếu `t* = t`, thông điệp được coi là toàn vẹn và xác thực. 
* **Mục đích:** Kẻ tấn công không biết khóa `k` sẽ không thể tạo ra một thẻ `t'` hợp lệ cho một thông điệp giả mạo `m'`. 

#### Đặc tính của MAC

Một hàm MAC an toàn cần có các đặc tính sau: 
* **Tính đúng đắn:** `V(k, m, S(k,m)) = True`. 
* **Tính hiệu quả:** Có thể tính toán nhanh chóng. 
* **Tính an toàn:** Nếu đối phương không biết khóa `k`: 
    * Không thể tính được `t` cho một `m` bất kỳ. 
    * Không thể tìm được một `m` tương ứng với một `t` cho trước. 
    * Không thể tìm được hai thông điệp `m` và `m'` khác nhau mà lại có cùng một giá trị MAC (tính chống đụng độ). 

#### MAC cung cấp dịch vụ nào?

* **Toàn vẹn:** CÓ. Kẻ tấn công không thể thay đổi thông điệp mà không bị phát hiện. 
* **Xác thực danh tính:** CÓ. Nếu Alice nhận được một thông điệp có MAC hợp lệ với khóa mà cô chỉ chia sẻ với Bob, cô có thể chắc chắn người gửi là Bob. 
* **Chống từ chối (Non-repudiation):** KHÔNG. Alice không thể chứng minh với một bên thứ ba rằng Bob đã gửi thông điệp, bởi vì chính Alice cũng có khóa bí mật và có thể đã tự tạo ra thông điệp đó. 
* **Bí mật:** KHÔNG. MAC không che giấu nội dung thông điệp; `m` vẫn được gửi ở dạng rõ. 

#### Xây dựng MAC: CBC-MAC

Một cách phổ biến để xây dựng MAC là từ một thuật toán mã khối (như AES) theo chế độ CBC.
* **Cách làm:** Mã hóa thông điệp bằng thuật toán mã khối ở chế độ CBC với khóa `k1` và vector khởi tạo (IV) bằng 0. Thẻ MAC chính là khối bản mã cuối cùng. 
* **An toàn:** Để đảm bảo an toàn (chống lại tấn công mở rộng độ dài - length extension attack), kết quả của bước trên cần được mã hóa thêm một lần nữa với một khóa thứ hai `k2`. 
* **Padding:** Khi thông điệp không chia hết cho kích thước khối, cần phải sử dụng một lược đồ đệm (padding) không gây nhập nhằng (ví dụ: chuẩn ISO/IEC 9797-1). 
* **Độ an toàn:** Độ an toàn của CBC-MAC giảm dần theo số lượng thông điệp được ký với cùng một khóa. Xác suất một kẻ tấn công có thể giả mạo thành công là tỉ lệ thuận với `q^2` (với `q` là số thông điệp đã ký). Do đó, cần phải thay khóa định kỳ sau khi đã ký một số lượng thông điệp nhất định.  Ví dụ, với AES-CBC-MAC, khóa nên được thay sau khi đã ký khoảng 256MB dữ liệu. 

#### Tấn công phát lại (Replay Attack)

* **Vấn đề:** MAC không tự chống lại được tấn công phát lại. Kẻ tấn công có thể ghi lại một cặp `(m, t)` hợp lệ và gửi lại nó ở một thời điểm khác.  Ví dụ, kẻ tấn công có thể phát lại một lệnh chuyển tiền đã thực hiện thành công. 
* **Giải pháp:** Các giao thức sử dụng MAC phải tự tích hợp các cơ chế chống phát lại, chẳng hạn như: 
    * **Giá trị dùng một lần (Nonce):** Thêm một số ngẫu nhiên chỉ dùng một lần vào thông điệp trước khi tính MAC: `S(k, m || nonce)`. 
    * **Tem thời gian (Timestamp):** Thêm thông tin thời gian vào thông điệp: `S(k, m || timestamp)`. 

#### Mật mã có xác thực (Authenticated Encryption - AE)

Để đảm bảo cả **tính bí mật** và **tính toàn vẹn**, người ta thường kết hợp mã hóa và MAC. Có ba cách kết hợp chính:

1.  **MAC-then-Encrypt (Dùng trong SSL/TLS):** Tính MAC của bản rõ, sau đó mã hóa cả (bản rõ + MAC). 
    `E(k_e, m || S(k_m, m))`
2.  **Encrypt-and-MAC (Dùng trong SSH):** Mã hóa bản rõ, đồng thời tính MAC trên bản rõ. Gửi cả hai đi. 
    `E(k_e, m) || S(k_m, m)`
3.  **Encrypt-then-MAC (Dùng trong IPSec):** Mã hóa bản rõ trước, sau đó tính MAC trên bản mã vừa tạo ra. 
    `c = E(k_e, m), t = S(k_m, c)`

Trong ba cách trên, **Encrypt-then-MAC** được chứng minh là an toàn nhất về mặt lý thuyết. 

**Lưu ý quan trọng:** Không bao giờ được tái sử dụng cùng một khóa cho cả hai mục đích mã hóa và xác thực. 

**AEAD (Authenticated Encryption with Additional Data):**
Là các thuật toán hiện đại được thiết kế để thực hiện đồng thời cả mã hóa và xác thực một cách hiệu quả và an toàn. 
* Chúng có thể xác thực cả những dữ liệu không cần mã hóa (ví dụ: header của gói tin). 
* **Ví dụ:** GCM (Galois/Counter Mode), CCM.