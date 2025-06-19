### 5. Xác thực bằng sinh trắc (Biometric Authentication)

Đây là phương pháp xác thực dựa trên yếu tố "cái mà chủ thể là" (What you are), sử dụng các đặc điểm sinh học hoặc hành vi độc nhất của một người để xác thực danh tính.

Một số phương pháp xác thực sinh trắc phổ biến bao gồm:
* Dấu vân tay 
* Vân lòng bàn tay 
* Cấu trúc hình học bàn tay 
* Khuôn mặt 
* Mống mắt 
* Vành tai 
* Mạch máu 

### 6. Single Sign-On (SSO)

#### 6.1. Khái niệm

* **Single Sign-On (SSO)** là một cơ chế xác thực cho phép người dùng **đăng nhập chỉ một lần** với một bộ tài khoản và mật khẩu duy nhất để có thể truy cập vào nhiều ứng dụng và dịch vụ khác nhau trong cùng một phiên làm việc mà không cần phải đăng nhập lại.
* Cơ chế này giúp cải thiện trải nghiệm người dùng và đơn giản hóa việc quản lý danh tính trong một môi trường có nhiều ứng dụng.

#### 6.2. Các giải pháp SSO

Có nhiều giải pháp và tiêu chuẩn SSO đang được sử dụng rộng rãi trong thực tế:
* OpenID Connect 
* CAS (Central Authentication Service) 
* Open SAML 
* CA Single Sign On 
* Java Open Single Sign On 
* Google Sign-In 
* Facebook Login 

#### 6.3. Vượt qua Xác thực đa yếu tố (MFA)

Ngay cả khi sử dụng các phương pháp xác thực mạnh như sinh trắc học hoặc OTP, hệ thống vẫn có thể bị tấn công nếu thiết kế không cẩn thận.

* **Tấn công chuyển tiếp (Relay Attack):** Đây là một kỹ thuật tinh vi mà kẻ tấn công sử dụng một trang web giả mạo để làm trung gian, chuyển tiếp thông tin xác thực giữa nạn nhân và trang web thật trong thời gian thực.
* **Kịch bản tấn công:**
    1.  Kẻ tấn công lừa nạn nhân truy cập vào một trang web giả mạo (ví dụ: `google-login.com` thay vì `google.com`).
    2.  Nạn nhân nhập tên người dùng và mật khẩu vào trang giả mạo.
    3.  Kẻ tấn công ngay lập tức sử dụng thông tin này để đăng nhập vào trang web thật.
    4.  Trang web thật yêu cầu yếu tố xác thực thứ hai (ví dụ: gửi mã OTP đến điện thoại của nạn nhân).
    5.  Trang web giả mạo hiển thị một ô yêu cầu nạn nhân nhập mã OTP mà họ vừa nhận được.
    6.  Nạn nhân nhập mã OTP vào trang giả mạo, và kẻ tấn công lại chuyển tiếp mã này đến trang web thật để hoàn tất quá trình đăng nhập.

Kết quả là kẻ tấn công đã vượt qua được hàng rào xác thực hai yếu tố và chiếm được quyền truy cập vào tài khoản của nạn nhân.