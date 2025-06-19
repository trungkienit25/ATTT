### 2. Xác thực bằng mật khẩu (Password Authentication)

Đây là phương pháp xác thực phổ biến nhất, dựa trên yếu tố "cái mà chủ thể biết". 

#### 2.1. Khái niệm và Các lỗ hổng

* **Khái niệm:** Mật khẩu là một chuỗi ký tự hoặc một nhóm từ được sử dụng để xác thực danh tính của một thực thể.  Quá trình này có hai bên tham gia: thực thể cần xác thực và người thẩm tra (hệ thống). 
* **Các lỗ hổng phổ biến:**
    * Lưu trữ mật khẩu trong cơ sở dữ liệu một cách không an toàn. 
    * Truyền mật khẩu trên kênh truyền không an toàn. 
    * Sự bất cẩn của người dùng. 

#### 2.2. Vấn đề từ phía người dùng

* Con người không thể nhớ được nhiều mật khẩu "mạnh" (dài, phức tạp). 
* Do đó, người dùng thường có xu hướng **sử dụng lại cùng một mật khẩu** cho nhiều tài khoản khác nhau với hy vọng sẽ không có điều gì tồi tệ xảy ra. 
* Đây là một rủi ro cực kỳ lớn. Khi mật khẩu của chỉ một tài khoản bị lộ (ví dụ: do cơ sở dữ liệu của một dịch vụ nào đó bị tấn công), kẻ tấn công sẽ có được mật khẩu của người dùng và dùng nó để đăng nhập vào tất cả các tài khoản quan trọng khác của họ (email, ngân hàng,...). 

* **Giải pháp cho người dùng:**
    * Sử dụng các **phần mềm quản lý mật khẩu** để tạo và lưu trữ các mật khẩu mạnh, duy nhất cho mỗi tài khoản. 
    * Sử dụng **thẻ xác thực hai yếu tố (U2F Security Keys)**. 
    * Kích hoạt tùy chọn **xác thực hai yếu tố (2FA)** trên các dịch vụ có hỗ trợ. 

#### 2.3. Tấn công vào Hệ xác thực bằng mật khẩu

* **Tấn công thụ động:**
    * Nhìn trộm (Shoulder surfing). 
    * Sử dụng chương trình ghi lại thao tác bàn phím (Keylogging). 
    * Chặn bắt gói tin trên mạng. 
* **Tấn công chủ động:**
    * **Tấn công Đoán và Thử (Guessing Attack):** Kẻ tấn công tương tác trực tiếp với hệ thống để thử các mật khẩu có thể.  Để giảm thiểu, hệ thống cần tăng độ phức tạp của mật khẩu và **hạn chế số lần thử xác thực** trong một khoảng thời gian. 
    * **Tấn công Lợi dụng Lỗ hổng Lưu trữ:** Đây là hình thức tấn công sau khi kẻ tấn công đã có được file cơ sở dữ liệu mật khẩu.

#### 2.4. Lưu trữ Mật khẩu An toàn

Việc lưu trữ mật khẩu phía máy chủ phải được thực hiện một cách cẩn mật để giảm thiểu thiệt hại khi cơ sở dữ liệu bị đánh cắp.

* **Lưu dạng rõ (Cleartext):** Tuyệt đối không được làm, đây là cách làm mất an toàn nhất. 
* **Lưu dạng băm (Hashed Password):** Đây là phương pháp được khuyến nghị. Thay vì lưu mật khẩu, hệ thống chỉ lưu giá trị băm của nó. 
    * **Vấn đề:** Dễ bị tấn công từ điển (Dictionary Attack) hoặc tấn công bảng cầu vồng (Rainbow Table Attack), trong đó kẻ tấn công đã tính toán trước giá trị băm của hàng triệu mật khẩu phổ biến. 
* **Sử dụng "Salt" để chống tấn công từ điển:**
    * **Salt** là một chuỗi ký tự ngẫu nhiên, duy nhất cho mỗi người dùng, được thêm vào mật khẩu trước khi băm. 
    * Hệ thống sẽ lưu trữ cặp `[salt, Hash(password || salt)]`. 
    * Vì mỗi người dùng có một salt khác nhau, kẻ tấn công không thể sử dụng các bảng băm đã tính toán trước. Hắn buộc phải tính toán lại từ điển của mình cho từng salt, khiến cuộc tấn công trở nên tốn kém hơn rất nhiều. 
* **Nâng cao an toàn hơn nữa:**
    * **Băm nhiều lần (Multiple Hashing Rounds):** Thực hiện băm lặp đi lặp lại hàng nghìn lần (`hash(hash(…))`) để làm chậm đáng kể quá trình tính toán của kẻ tấn công. 
    * **Sử dụng "Pepper":** Thêm một chuỗi bí mật (pepper) vào trước khi băm. Pepper này giống nhau cho tất cả người dùng và được lưu ở một nơi khác, tách biệt với cơ sở dữ liệu (ví dụ: trong file cấu hình). Điều này ngăn chặn kẻ tấn công có thể thực hiện tấn công từ điển ngay cả khi đã chiếm được cơ sở dữ liệu và salt. 
    * **Sử dụng các thuật toán chuyên dụng:** Các thuật toán như **bcrypt, scrypt, PBKDF2** được thiết kế đặc biệt cho việc băm mật khẩu, tích hợp sẵn các cơ chế như salt và băm nhiều lần, và được coi là lựa chọn an toàn nhất hiện nay. 

#### 2.5. Khôi phục mật khẩu và Chính sách mật khẩu

* **Khôi phục mật khẩu:** Các cơ chế khôi phục (gửi link qua email, câu hỏi bí mật,...) phải được thiết kế cẩn thận để không trở thành điểm yếu của hệ thống.  Cơ chế "câu hỏi bí mật" không còn được coi là an toàn do thông tin cá nhân của người dùng ngày càng dễ bị tìm thấy trên mạng xã hội. 
* **Chính sách mật khẩu:** Các tổ chức nên áp dụng các chính sách mật khẩu mạnh (quy định độ dài, độ phức tạp, thay đổi định kỳ,...) để tăng cường an toàn, tuy nhiên luôn phải cân bằng với tính tiện lợi cho người dùng.