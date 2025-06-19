### 4. Mật khẩu một lần (One-Time Password - OTP)

#### 4.1. Nhu cầu về Xác thực đa yếu tố (MFA)

* Việc xác thực chỉ dựa trên mật khẩu truyền thống không còn đủ an toàn. Người dùng thường gặp khó khăn trong việc tạo và nhớ các mật khẩu mạnh, duy nhất cho mỗi tài khoản, trong khi kẻ tấn công có thể thực hiện các cuộc tấn công từ điển nếu cơ sở dữ liệu bị lộ.
* **Xác thực đa yếu tố (MFA)** ra đời để giải quyết vấn đề này. MFA yêu cầu người dùng kết hợp ít nhất hai yếu tố xác thực khác nhau, ví dụ:
    * **Cái bạn biết** (mật khẩu).
    * **Cái bạn có** (điện thoại để nhận OTP).
* Với MFA, việc kẻ tấn công chỉ biết mật khẩu là không đủ để vượt qua hàng rào xác thực.

#### 4.2. Khái niệm OTP

* **Định nghĩa:** OTP là một mật khẩu chỉ có giá trị hợp lệ cho **một lần đăng nhập hoặc một giao dịch duy nhất**.
* **Phân loại:**
    * S/Key OTP.
    * **HOTP (Hash-based OTP):** Dựa trên sự kiện.
    * **TOTP (Time-based OTP):** Dựa trên thời gian.
* **Phương thức phân phối:** OTP có thể được phân phối cho người dùng qua SMS, ứng dụng di động (như Google Authenticator), email, hoặc thiết bị phần cứng chuyên dụng (token).

#### 4.3. Các loại OTP

##### HOTP (Hash-based One-Time Password - RFC 4226)
HOTP tạo ra mã OTP dựa trên một khóa bí mật và một bộ đếm.

* **Thành phần:**
    * **Khóa bí mật `K`:** Được chia sẻ an toàn giữa người dùng và máy chủ.
    * **Bộ đếm `C`:** Là một bộ đếm dựa trên sự kiện, tăng lên mỗi khi một mã OTP mới được tạo ra.
* **Quy trình tạo mã:** `HOTP(K, C) = Truncate(HMAC-SHA-1(K, C))`.
    1.  Tính giá trị băm `HMAC-SHA-1` từ khóa `K` và bộ đếm `C`.
    2.  Sử dụng kỹ thuật **Dynamic Truncation** để trích xuất một chuỗi 4 byte từ kết quả băm.
    3.  Chuyển chuỗi 4 byte này thành một số và lấy `k` chữ số cuối cùng (thường là 6) làm mã OTP.
* **Vấn đề đồng bộ:** Nếu người dùng tạo mã OTP trên thiết bị của mình nhưng không sử dụng nó để đăng nhập, bộ đếm trên thiết bị và máy chủ sẽ bị lệch nhau (mất đồng bộ). Máy chủ cần có cơ chế dò tìm trong một khoảng cho phép để đồng bộ lại với bộ đếm của người dùng.

##### TOTP (Time-based One-Time Password - RFC 6238)
TOTP là một cải tiến của HOTP và là loại OTP phổ biến nhất hiện nay.

* **Nguyên tắc:** Thay vì dùng bộ đếm sự kiện, TOTP sử dụng một **bộ đếm dựa trên thời gian**.
* **Công thức tính bộ đếm `C`:**
    `C = (Thời_gian_Unix_hiện_tại – T0) / X`
    Trong đó, `T0` là mốc thời gian ban đầu và `X` là bước thời gian (thường là 30 giây).
* **Hoạt động:** Mã OTP sẽ tự động thay đổi sau mỗi bước thời gian `X`.
* **Vấn đề đồng bộ:** Do có thể có độ trễ mạng hoặc sự chênh lệch đồng hồ nhỏ giữa máy khách và máy chủ, phía máy chủ thường cho phép chấp nhận các mã OTP được tạo ra trong một khoảng thời gian sai số nhỏ (ví dụ: chấp nhận mã của chu kỳ trước đó và chu kỳ kế tiếp).

#### 4.4. OTP qua SMS

* **Cách thức:** Mã OTP được sinh ra ở máy chủ và gửi cho người dùng thông qua tin nhắn SMS.
* **Rủi ro an toàn:** Phương pháp này hiện được coi là **không đảm bảo an toàn** vì những lý do sau:
    * Điện thoại của người dùng có thể bị nhiễm mã độc nghe lén tin nhắn.
    * Kẻ tấn công có thể giả mạo trạm thu phát sóng di động (BTS) để chặn bắt tin nhắn.
    * Kẻ tấn công có thể khai thác các lỗ hổng nghiêm trọng trong **giao thức SS7** (hệ thống báo hiệu lõi của mạng di động) để chuyển hướng và nhận tin nhắn SMS của nạn nhân.