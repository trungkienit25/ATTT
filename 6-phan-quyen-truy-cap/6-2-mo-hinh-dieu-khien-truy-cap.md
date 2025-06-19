### 2. Các mô hình điều khiển truy cập

Để triển khai việc điều khiển truy cập, người ta đã phát triển các mô hình chính sách khác nhau, mỗi mô hình có ưu, nhược điểm và phạm vi ứng dụng riêng.

#### 2.1. Mô hình Điều khiển truy cập Tùy nghi (Discretionary Access Control - DAC)

* **Định nghĩa:** Trong mô hình DAC, quyền truy cập trên một tài nguyên được quyết định bởi chính **chủ sở hữu (owner)** của tài nguyên đó. Chủ sở hữu có toàn quyền (discretion) trong việc cấp phát hoặc thu hồi quyền truy cập cho các chủ thể khác.
* **Ứng dụng:** Mô hình này được sử dụng rộng rãi trong các hệ điều hành thương mại như Windows, Linux, hoặc trong các hệ quản trị cơ sở dữ liệu như SQL. Ví dụ, người dùng có thể tự quyết định chia sẻ tệp tin hoặc thư mục của mình cho người khác.
* **Ví dụ trong SQL:** Lệnh `GRANT` cho phép người dùng cấp quyền, và tùy chọn `WITH GRANT OPTION` cho phép người được cấp quyền có thể tiếp tục cấp phát quyền đó cho người khác nữa.
* **Hạn chế:** Khả năng quản trị của DAC khá lỏng lẻo. Nó không kiểm soát được sự lan truyền của quyền, có thể dẫn đến việc thông tin bị rò rỉ ngoài ý muốn và gây mất an toàn cho hệ thống. Ví dụ, Alice có quyền đọc file A, cô ấy có thể dễ dàng sao chép nội dung file A sang file B và cấp quyền đọc file B cho Bob, người vốn không có quyền đọc file A.

#### 2.2. Mô hình Điều khiển truy cập Cưỡng bức (Mandatory Access Control - MAC)

* **Định nghĩa:** Trong mô hình MAC, quyền truy cập được cấp phát dựa trên một **chính sách chung của toàn hệ thống**, thay vì do chủ sở hữu quyết định.
* **Cách hoạt động:** Hệ thống sẽ phân loại cả chủ thể (dựa trên mức độ tin cậy) và tài nguyên (dựa trên mức độ nhạy cảm), sau đó áp dụng các quy tắc truy cập một cách cưỡng bức.
* **Ưu điểm:** Quản trị tập trung, tính bảo mật cao, phù hợp cho các môi trường yêu cầu an ninh nghiêm ngặt như quân đội, chính phủ.
* **Nhược điểm:** Kém linh hoạt, đòi hỏi phải phân loại rõ ràng tất cả các chủ thể và tài nguyên, phạm vi ứng dụng hạn chế.
* **Hai mô hình MAC kinh điển:**
    * **Mô hình Bell-LaPadula (Bảo vệ tính bí mật):**
        * Áp dụng quy tắc "Không đọc trên, không ghi dưới" (No-read-up, no-write-down). Một chủ thể ở mức độ bí mật `Confidential` có thể đọc tài liệu `Confidential` hoặc `Unclassified`, nhưng không thể đọc tài liệu `Secret` (no-read-up). Đồng thời, chủ thể đó không được phép ghi thông tin vào một tài liệu có mức độ bí mật thấp hơn (ví dụ: `Unclassified`) để tránh làm rò rỉ thông tin (no-write-down).
    * **Mô hình Biba (Bảo vệ tính toàn vẹn):**
        * Áp dụng quy tắc ngược lại: "Không đọc dưới, không ghi trên" (No-read-down, no-write-up). Một chủ thể chỉ có thể ghi vào các tài nguyên có mức độ toàn vẹn bằng hoặc thấp hơn mình (để tránh làm "bẩn" dữ liệu quan trọng hơn) và chỉ có thể đọc các tài nguyên có mức độ toàn vẹn bằng hoặc cao hơn mình (để không bị ảnh hưởng bởi dữ liệu kém tin cậy).

#### 2.3. Mô hình Điều khiển truy cập theo Vai trò (Role-Based Access Control - RBAC)

* **Định nghĩa:** Trong mô hình RBAC, việc cấp quyền truy cập không hướng trực tiếp tới người dùng cuối mà hướng tới các **vai trò (Role)** trong hệ thống.
* **Khái niệm:**
    * **Vai trò (Role):** Là một khái niệm tượng trưng cho một nhóm người dùng có cùng nhiệm vụ hoặc chức năng trong một tổ chức (ví dụ: Kế toán, Nhân sự, Quản trị viên).
    * Quyền truy cập sẽ được gán cho các vai trò.
    * Người dùng sẽ được gán vào một hoặc nhiều vai trò và từ đó thừa hưởng các quyền truy cập tương ứng.
* **Ưu điểm:**
    * Phản ánh tốt hơn đặc trưng nghiệp vụ của các tổ chức.
    * **Linh hoạt và dễ quản lý:** Khi có sự thay đổi về chính sách, quản trị viên chỉ cần sửa quyền trên vai trò thay vì phải sửa cho từng người dùng. Khi có nhân viên mới, chỉ cần gán họ vào vai trò phù hợp.
    * **Có khả năng mở rộng tốt** và hỗ trợ các chính sách phức tạp như phân chia nhiệm vụ.
* **Các cấp độ RBAC:**
    * **RBAC0 (Core RBAC):** Mô hình cơ bản nhất, bao gồm các quan hệ Người dùng-Vai trò và Quyền-Vai trò.
    * **RBAC1 (Hierarchical RBAC):** Cho phép tổ chức các vai trò theo dạng phân cấp (cây thừa kế). Vai trò ở cấp cao hơn sẽ được thừa hưởng các quyền của vai trò ở cấp thấp hơn.