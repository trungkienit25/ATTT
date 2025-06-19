# Bài 6: Phân quyền truy cập (Access Control)

### 1. Khái niệm cơ bản

#### 1.1. Định nghĩa và Mô hình AAA

* **Điều khiển truy cập (Access Control):** Là một chức năng của hệ thống được thi hành để cho phép một chủ thể (người dùng, tiến trình, thiết bị) được truy cập đến một tài nguyên của hệ thống với một mức độ nhất định (quyền truy cập), cũng như chia sẻ quyền truy cập này cho chủ thể khác. Chức năng này có mặt trong hầu hết các ứng dụng và hệ thống công nghệ thông tin.
* **Mô hình AAA:** Quá trình điều khiển truy cập thường được mô tả qua mô hình AAA, bao gồm ba thành phần chính:
    * **Authentication (Xác thực):** Xác định đúng danh tính của chủ thể đang thực hiện hành vi truy cập.
    * **Authorization (Ủy quyền):** Quyết định và cấp phát các quyền truy cập cho chủ thể đó trên tài nguyên cụ thể.
    * **Auditing (Kiểm toán):** Ghi lại, kiểm tra và giám sát các hành vi truy cập đã xảy ra.

#### 1.2. Kiểm soát hoàn toàn và Reference Monitor

* Để đảm bảo an toàn, mọi truy cập tới tài nguyên đều phải đi qua một module kiểm tra quyền gọi là **Reference Monitor**.
* Một Reference Monitor hiệu quả phải thỏa mãn ba điều kiện:
    * **Không thể vòng tránh (Unbypassable):** Mọi truy cập đều phải đi qua nó.
    * **Chống sửa đổi (Tamper-resistant):** Bản thân nó phải được bảo vệ khỏi sự thay đổi trái phép.
    * **Có thể thẩm tra (Verifiable):** Thiết kế của nó phải đủ đơn giản để có thể kiểm tra và chứng minh tính đúng đắn.
* Do các yêu cầu khắt khe này, Reference Monitor được coi là một thể hiện của **Cơ sở tính toán được tin cậy (TCB)**.

#### 1.3. Ma trận điều khiển truy cập (Access Control Matrix - ACM)

* **Ma trận điều khiển truy cập (ACM)** là một mô hình lý thuyết để thể hiện các quyền đã được cấp phát cho các chủ thể (subject) trên từng tài nguyên (object) của hệ thống.
* Một ma trận ACM có các dòng đại diện cho chủ thể `S`, các cột đại diện cho tài nguyên `O`, và ô `A(si, oj)` chứa danh sách các quyền truy cập `R` mà chủ thể `si` có trên tài nguyên `oj`.

#### 1.4. Các phương pháp cài đặt ACM

Việc cài đặt trực tiếp một ma trận ACM đầy đủ là không khả thi trong các hệ thống lớn vì số lượng tài nguyên và chủ thể là rất lớn, làm cho ma trận trở nên cồng kềnh và thưa thớt. Do đó, người ta sử dụng các phương pháp cài đặt gián tiếp bằng cách phân rã ma trận.

1.  **Danh sách điều khiển truy cập (Access Control List - ACL):**
    * Đây là phương pháp **phân rã ma trận theo cột**, tiếp cận theo hướng tài nguyên.
    * Mỗi tài nguyên sẽ được gắn với một danh sách ACL, trong đó liệt kê các chủ thể và quyền truy cập tương ứng của họ trên tài nguyên đó.
    * Phương pháp này đòi hỏi phải xác thực danh tính của chủ thể mỗi khi có yêu cầu truy cập. Các hệ điều hành như Windows hay Linux sử dụng ACL để phân quyền trên tệp tin và thư mục.

2.  **Danh sách năng lực (Capability List - CL):**
    * Đây là phương pháp **phân rã ma trận theo dòng**, tiếp cận theo hướng chủ thể.
    * Mỗi chủ thể sẽ sở hữu một danh sách năng lực, trong đó liệt kê các tài nguyên mà chủ thể đó có thể truy cập và các quyền tương ứng.
    * Danh sách này thường được triển khai dưới dạng các **thẻ truy cập (capability/token)** mà không cần chứa danh tính của chủ thể.
    * Khi một chủ thể trình ra một "năng lực" hợp lệ, hệ thống sẽ cho phép truy cập mà không cần xác thực lại danh tính. Vấn đề cần giải quyết với phương pháp này là chống giả mạo và chống sử dụng lại trái phép các thẻ năng lực.