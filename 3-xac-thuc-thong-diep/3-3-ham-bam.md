### 3. Hàm băm và HMAC

Sau khi tìm hiểu về MAC, chúng ta sẽ xem xét một công cụ mật mã nền tảng khác thường được dùng để xây dựng MAC và nhiều ứng dụng khác: **Hàm băm**.

#### 3.1. Khái niệm Hàm băm

* **Định nghĩa:** Hàm băm `H` là một thuật toán nhận đầu vào là một bản tin `m` có kích thước bất kỳ và tạo ra một đầu ra `h` có kích thước cố định, được gọi là **giá trị băm (hash value)** hay **bản tóm lược (digest)**.
    `h = H(m)`

* **Đặc tính của một hàm băm mật mã:** Một hàm băm được sử dụng cho mục đích an ninh, bảo mật phải có các đặc tính sau:
    1.  **Tính hiệu quả:** Dễ dàng tính toán `h` từ `m`.
    2.  **Tính một chiều (Preimage Resistance):** Cho trước giá trị băm `h`, việc tìm ra bản tin gốc `m` sao cho `H(m) = h` phải cực kỳ khó khăn về mặt tính toán.
    3.  **Tính chống đụng độ yếu (Second Preimage Resistance):** Cho trước bản tin `m1`, việc tìm ra một bản tin `m2` khác (`m2 ≠ m1`) sao cho `H(m1) = H(m2)` phải cực kỳ khó khăn.
    4.  **Tính chống đụng độ mạnh (Collision Resistance):** Việc tìm ra một cặp hai bản tin bất kỳ `m1` và `m2` khác nhau mà có cùng giá trị băm phải cực kỳ khó khăn.
    5.  **Tính ngẫu nhiên (Avalanche Effect):** Một thay đổi dù là nhỏ nhất ở đầu vào (ví dụ: thay đổi 1 bit) phải tạo ra một sự thay đổi lớn và không thể đoán trước được ở đầu ra (thay đổi khoảng 50% số bit của giá trị băm).

#### 3.2. Tấn công vào Hàm băm và Nghịch lý ngày sinh

Độ an toàn của hàm băm phụ thuộc vào độ khó của việc tấn công các đặc tính trên, và điều này liên quan trực tiếp đến kích thước của giá trị băm đầu ra (`n` bit).

* **Tấn công vào tính một chiều và chống đụng độ yếu:** Cần khoảng **$2^n$** phép tính để có thể tìm ra bản tin gốc từ một giá trị băm cho trước bằng phương pháp vét cạn.
* **Tấn công vào tính chống đụng độ mạnh (Birthday Attack):** Việc tìm ra hai bản tin bất kỳ có cùng giá trị băm thì dễ hơn đáng kể. Dựa trên **Nghịch lý ngày sinh (Birthday Paradox)**, kẻ tấn công chỉ cần khoảng **$2^{n/2}$** phép tính để có thể tìm ra một vụ "đụng độ" (collision) với xác suất thành công cao.

> **Nghịch lý ngày sinh:** Trong một nhóm chỉ có 23 người, xác suất để có ít nhất hai người trùng ngày sinh đã lớn hơn 50%. Tương tự, để tìm ra hai bản tin có cùng giá trị băm n-bit, ta không cần thử $2^n$ bản tin mà chỉ cần khoảng $2^{n/2}$ bản tin.

Do cuộc tấn công Birthday, để đảm bảo an toàn, kích thước đầu ra của hàm băm phải đủ lớn. Ví dụ, một hàm băm có đầu ra 128 bit (như MD5) chỉ cung cấp mức an toàn chống đụng độ tương đương $2^{64}$, một con số không còn an toàn với sức mạnh tính toán hiện nay.

#### 3.3. Một số hàm băm phổ biến

* **MD5:** Tạo ra giá trị băm 128 bit. Đã bị tấn công đụng độ thành công từ năm 2005 và được coi là **hoàn toàn không an toàn** cho các ứng dụng mật mã.
* **SHA-1:** Tạo ra giá trị băm 160 bit. Đã bị tấn công đụng độ thành công vào năm 2017 và hiện đã bị loại bỏ trong hầu hết các ứng dụng bảo mật.
* **SHA-2:** Là một họ các hàm băm (SHA-224, SHA-256, SHA-384, SHA-512) và hiện vẫn được coi là an toàn.
* **SHA-3:** Là một chuẩn mới được phát hành để làm phương án thay thế cho SHA-2, với cấu trúc bên trong hoàn toàn khác (cấu trúc bọt biển - sponge construction) để tăng tính đa dạng cho các thuật toán.

#### 3.4. Sử dụng hàm băm và HMAC

* **Xác thực toàn vẹn:** Hàm băm thường được dùng để kiểm tra tính toàn vẹn của dữ liệu, ví dụ như mã băm của các file tải về trên website.
* **Xây dựng MAC:** Để xác thực thông điệp trong môi trường có kẻ tấn công chủ động, ta không thể chỉ băm bản tin `H(m)` vì kẻ tấn công có thể sửa bản tin và tính lại giá trị băm mới. Ta cần kết hợp một khóa bí mật `k` vào quá trình băm.
* **Vấn đề với `H(k || m)`:** Việc băm trực tiếp `khóa + thông điệp` là không an toàn với các hàm băm có cấu trúc Merkle-Damgård (như MD5, SHA-1, SHA-2) vì chúng dễ bị **tấn công mở rộng độ dài (Length Extension Attack)**. Kẻ tấn công có thể tính `H(k || m || dữ_liệu_thêm)` mà không cần biết khóa `k`.

#### HMAC: Hashed MAC

Để giải quyết vấn đề trên, chuẩn **HMAC** đã được ra đời. Đây là một phương pháp xây dựng MAC từ một hàm băm bất kỳ một cách an toàn.

* **Công thức:** `HMAC(k, m) = H((k ⊕ opad) || H((k ⊕ ipad) || m))`
* **Nguyên tắc:** HMAC sử dụng khóa hai lần trong quá trình tính toán (một lần ở trong, một lần ở ngoài). Cấu trúc "băm lồng nhau" này giúp HMAC chống lại được tấn công mở rộng độ dài.
* HMAC được sử dụng rộng rãi trong nhiều giao thức bảo mật như IPSec và TLS.