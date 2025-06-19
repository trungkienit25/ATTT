# Bài 5: Xác thực danh tính (Identity Authentication)

### 1. Khái niệm chung

#### 1.1. Xác thực danh tính là gì?

Xác thực danh tính là quá trình trả lời cho câu hỏi "Ai đang truy cập hoặc trao đổi dữ liệu?". Khi một chủ thể (người dùng, thiết bị,...) thực hiện truy cập, hệ thống cần phải có cơ chế để xác minh được mối liên kết giữa chủ thể đó ở thế giới thực với danh tính mà họ đang sử dụng để truy cập.

#### 1.2. Các phương pháp xác thực

Một phương pháp xác thực thường dựa trên một hoặc nhiều yếu tố (factors) sau:

* **Cái chủ thể biết (What the entity knows):** Đây là những thông tin bí mật mà chỉ chủ thể biết, ví dụ như mật khẩu hoặc mã PIN.
* **Cái chủ thể có (What the entity has):** Đây là những vật thể mà chủ thể sở hữu, ví dụ như thẻ thông minh (smart card), USB token, hoặc điện thoại di động để nhận mã OTP.
* **Cái chủ thể là (What the entity is):** Đây là những đặc điểm sinh trắc học hoặc hành vi độc nhất của chủ thể, ví dụ như dấu vân tay, khuôn mặt, mống mắt, hoặc giọng nói.
* **Vị trí của chủ thể (Where the entity is):** Yếu tố này dựa trên vị trí địa lý của chủ thể tại thời điểm xác thực, ví dụ như qua địa chỉ IP hoặc định vị GPS.

#### 1.3. Xác thực đa yếu tố (Multi-Factor Authentication - MFA)

Xác thực đa yếu tố là phương pháp yêu cầu người dùng cung cấp từ hai yếu tố xác thực trở lên từ các nhóm khác nhau ở trên để có thể truy cập vào hệ thống. Ví dụ, một hệ thống MFA phổ biến yêu cầu người dùng nhập mật khẩu (cái bạn biết) và sau đó nhập mã OTP từ điện thoại (cái bạn có). Việc này giúp tăng cường đáng kể độ an toàn so với việc chỉ sử dụng một yếu tố duy nhất.