### 3. Case study: Điều khiển truy cập trong hệ điều hành Unix

Hệ điều hành Unix (và các hệ điều hành tương tự như Linux) có một cơ chế điều khiển truy cập kinh điển và hiệu quả, kết hợp nhiều mô hình khác nhau.

#### 3.1. Các khái niệm cơ bản

* **Định danh:**
    * Mỗi người dùng có một **UID (User ID)** duy nhất. 
    * Mỗi nhóm người dùng có một **GID (Group ID)** duy nhất. 
    * Mỗi tiến trình đang chạy có một **PID (Process ID)** duy nhất. 
* **Đối tượng cần điều khiển truy cập:** Là các **tệp tin (file)** và **thư mục (directory)**.  Unix coi mọi thứ, kể cả các thiết bị ngoại vi, là một tệp tin. 
* **Cấu trúc:** Tệp tin và thư mục được tổ chức theo cấu trúc phân cấp dạng cây. 

#### 3.2. Mô hình điều khiển truy cập của Unix

* Unix sử dụng một mô hình kết hợp giữa **DAC** (chủ sở hữu quyết định quyền) và **RBAC** (phân quyền theo nhóm), được triển khai dưới dạng một danh sách ACL rút gọn. 
* **Các quyền cơ bản:**
    * **Đọc (r - Read):** Đọc nội dung tệp tin hoặc liệt kê các tệp tin trong thư mục. 
    * **Ghi (w - Write):** Sửa đổi nội dung tệp tin hoặc tạo/xóa tệp tin trong thư mục. 
    * **Thực thi (x - Execute):** Chạy một tệp tin như một chương trình hoặc truy cập vào một thư mục. 
* **Biểu diễn quyền:**
    * Quyền truy cập được lưu trữ trong 9 bit, chia thành 3 nhóm, mỗi nhóm 3 bit (rwx). 
    * **Owner:** Quyền của người dùng sở hữu tệp tin. 
    * **Group:** Quyền của nhóm người dùng sở hữu tệp tin. 
    * **Other:** Quyền của tất cả những người dùng khác. 
    * **Ví dụ:** Quyền `rwx r-x --x` (dạng chuỗi) sẽ được biểu diễn bằng số là `751` (dạng bát phân). 
* **Các lệnh quản lý:**
    * `chmod`: Thay đổi quyền truy cập trên tệp tin/thư mục. 
    * `chown`: Thay đổi chủ sở hữu và nhóm sở hữu. 

#### 3.3. Các quyền đặc biệt: setuid và sticky bit

* **Vấn đề:** Làm thế nào một người dùng thông thường có thể thực thi một chương trình đòi hỏi quyền cao hơn (ví dụ: lệnh `passwd` cần sửa đổi file `/etc/shadow` mà chỉ `root` mới có quyền ghi)? 
* **Giải pháp: bit `setuid`**
    * Khi một bit `setuid` được bật trên một tệp tin thực thi, tiến trình chạy tệp tin đó sẽ có **quyền của chủ sở hữu tệp tin**, chứ không phải quyền của người đã thực thi nó. 
    * Mỗi tiến trình có 3 loại UID: **Real UID** (UID của người thực thi), **Effective UID** (UID hiệu lực dùng để kiểm tra quyền, sẽ thay đổi nếu có bit `setuid`), và **Saved UID**. 
* **Vấn đề:** Ai có thể xóa file của người khác trong một thư mục chung (như `/tmp`)? 
* **Giải pháp: `sticky bit`**
    * Khi `sticky bit` được bật trên một thư mục, chỉ chủ sở hữu của tệp tin (hoặc `root`) mới có quyền xóa hoặc đổi tên tệp tin đó, ngay cả khi những người dùng khác có quyền ghi trên thư mục. 

#### 3.4. MAC trong Linux: SELinux

* Để tăng cường an ninh, Linux cung cấp một module kernel gọi là **SELinux (Security-Enhanced Linux)**, có chức năng thiết lập các chính sách **Điều khiển truy cập Cưỡng bức (MAC)**. 
* **Các chế độ hoạt động:**
    * **Enforcing:** Chế độ mặc định, thực thi chính sách bảo mật một cách nghiêm ngặt. 
    * **Permissive:** Không thực thi chính sách, chỉ ghi lại các cảnh báo vi phạm. 
    * **Disabled:** Vô hiệu hóa hoàn toàn SELinux. 

#### 3.5. Hạn chế của mô hình Unix

* Nhiều dịch vụ mạng (network daemon) như `sshd`, `ftpd` có thể phải chạy với quyền `root`, tạo ra rủi ro an ninh. 
* Kẻ tấn công có thể thay đổi các biến môi trường như `LIBPATH` để lừa hệ thống nạp các thư viện độc hại. 
* Lỗ hổng **TOCTTOU (Time Of Check To Time Of Use):** Đây là một dạng lỗ hổng điều kiện tranh đua (race condition). Ví dụ, một tiến trình có quyền `root` kiểm tra sự tồn tại của file `/tmp/X` (an toàn), nhưng ngay sau đó và trước khi tiến trình mở file, kẻ tấn công đã kịp thay thế `/tmp/X` bằng một liên kết tượng trưng (symbolic link) trỏ đến file nhạy cảm `/etc/shadow`. Kết quả là tiến trình sẽ mở file `/etc/shadow` với quyền `root`.