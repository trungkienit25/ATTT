# Bài 4: Quản lý và phân phối khóa

### 1. Giới thiệu chung về giao thức mật mã

#### 1.1. Giao thức mật mã là gì?

* Việc hiểu về các công cụ mật mã (mã hóa, chữ ký số,...) là chưa đủ để xây dựng một hệ thống an toàn. Chúng ta cần phải trả lời câu hỏi "Sử dụng các công cụ đó như thế nào?".
* Một hệ mật mã dù an toàn đến đâu cũng sẽ trở nên vô dụng nếu giao thức sử dụng nó được thiết kế tồi, đặc biệt khi phải tính đến khả năng các bên tham gia không trung thực.
* **Giao thức (Protocol):** Là một chuỗi các bước được định nghĩa trước mà các bên tham gia phải thực hiện để hoàn thành một tác vụ nào đó. Giao thức cũng quy định cả quy cách biểu diễn thông tin được trao đổi.
* **Giao thức mật mã (Cryptographic Protocol):** Là một giao thức có sử dụng các cơ chế, thuật toán mật mã để đạt được các mục tiêu về an toàn, bảo mật.

#### 1.2. Các đặc tính và yêu cầu

* **Đặc tính chung của một giao thức:**
    * Các bên tham gia phải hiểu về các bước thực hiện giao thức.
    * Các bên phải đồng ý tuân thủ chặt chẽ các bước thực hiện.
    * Giao thức phải được định nghĩa rõ ràng, không có sự nhập nhằng.
    * Giao thức phải định nghĩa đầy đủ hành động cho mọi tình huống có thể xảy ra.
* **Đặc tính riêng của giao thức mật mã:** Không một bên nào có thể thu được nhiều lợi ích hơn so với những gì đã được thiết kế trong giao thức.
* **Các yêu cầu an toàn quan trọng:**
    * **Forward Secrecy (Bảo mật hướng tới):** Dữ liệu của các phiên đã thực hiện trong quá khứ không thể được sử dụng để tấn công hay làm lộ thông tin của các phiên trong tương lai.
    * **Backward Secrecy (Bảo mật hướng về):** Dữ liệu của các phiên đã thực hiện vẫn được đảm bảo an toàn nếu các phiên trong tương lai bị tấn công thỏa hiệp.
    * **Perfect Forward Secrecy (trong phân phối khóa):** Khóa ngắn hạn (khóa phiên) vẫn phải an toàn ngay cả khi khóa dài hạn (ví dụ: khóa cá nhân) bị lộ trong tương lai.

#### 1.3. Các loại giao thức

1.  **Giao thức có trọng tài (Protocols with a Trusted Arbitrator):**
    * Sử dụng một bên thứ ba (trọng tài) được tất cả các bên tin tưởng tuyệt đối.
    * **Đặc điểm của trọng tài:** Không có quyền lợi riêng, không thiên vị, và luôn hoàn thành đúng, đủ nhiệm vụ.
    * **Ví dụ:** Alice và Bob muốn trao đổi máy tính và séc một cách an toàn, họ có thể nhờ một bên trung gian là Trent để thực hiện việc trao đổi.
    * **Hạn chế:** Tăng chi phí và độ trễ, trọng tài có thể trở thành "nút cổ chai" của hệ thống và cũng là một mục tiêu tấn công.

2.  **Giao thức có người phân xử (Adjudicated Protocols):**
    * Là một giải pháp linh hoạt hơn, chia giao thức thành hai phần:
        * Giao thức không cần trọng tài cho hoạt động thông thường.
        * Giao thức cần người phân xử, chỉ được kích hoạt khi có tranh chấp xảy ra.
    * **Ví dụ:** Alice và Bob ký hợp đồng với nhau. Nếu có tranh chấp, họ mới trình bằng chứng (hợp đồng đã ký) cho người phân xử (ví dụ: tòa án).

3.  **Giao thức tự phân xử (Self-Enforcing Protocols):**
    * Là loại giao thức không cần đến bất kỳ bên thứ ba nào.
    * Bản thân giao thức được thiết kế với các cơ chế để một bên có thể tự phát hiện hành vi gian lận của bên còn lại.
    * Các giao thức này thường rất phức tạp và không phải lúc nào cũng có thể thiết kế được. Ví dụ điển hình là các giao thức trong mạng Bitcoin.

#### 1.4. Các dạng tấn công vào giao thức mật mã

Kẻ tấn công có thể lợi dụng các điểm yếu trong cả hệ mật mã lẫn các bước thực hiện của giao thức.
* **Tấn công thụ động:** Nghe lén (eavesdropping) để thu thập thông tin.
* **Tấn công chủ động:** Can thiệp trực tiếp vào các luồng thông tin của giao thức.
    * Chèn các thông điệp giả mạo.
    * Thay thế nội dung của các thông điệp.
    * Sử dụng lại các thông điệp cũ (tấn công phát lại - replay attack).
    * Giả mạo danh tính của một trong các bên tham gia.