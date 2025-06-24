# Bài 1: Tổng quan về An toàn an ninh Thông tin

## I. Giới thiệu chung và Tầm quan trọng

### 1. An toàn an ninh thông tin là gì?

* An toàn an ninh thông tin (ATANTT) là việc bảo vệ các tài nguyên của hệ thống thông tin (phần cứng, phần mềm, dữ liệu, người dùng) trước các hành vi gây tổn hại.
* Các hành vi này bao gồm tấn công vật lý (vào phần cứng) và tấn công logic (can thiệp vào xử lý và truyền dữ liệu).
* Về bản chất, ATANTT là một dạng của **tính đúng đắn**, đảm bảo hệ thống có khả năng phát hiện, ngăn chặn các giá trị đầu vào không mong muốn và hoạt động chính xác ngay cả khi có sự hiện diện của kẻ tấn công.

### 2. Tại sao ATANTT lại quan trọng?

Các cuộc tấn công mạng ngày càng phổ biến và không trừ một thiết bị nào, từ máy tính cho đến các vật dụng thông minh như tủ lạnh cũng có thể bị hack để tấn công doanh nghiệp. Tác động của chúng rất sâu rộng:

* **An toàn thân thể:** Đã có những cảnh báo và sự việc thực tế cho thấy hacker có thể tấn công các thiết bị y tế như máy tạo nhịp tim hay máy bơm insulin, đe dọa trực tiếp đến tính mạng con người.
* **Bí mật thông tin:** Các vụ đánh cắp dữ liệu cá nhân quy mô lớn xảy ra liên tục, ảnh hưởng đến hàng trăm nghìn khách hàng của Vietnam Airlines, hàng triệu tài khoản Zing ID, và khiến các công ty lớn như British Airways bị phạt nặng. Ngay cả thông tin ngân hàng cũng là mục tiêu béo bở.
* **Tài sản:** Nhiều người dùng đã bị hacker đánh cắp số tiền lớn từ tài khoản ngân hàng một cách dễ dàng. Chi phí trung bình toàn cầu cho một vụ vi phạm dữ liệu vào năm 2022 là 4.35 triệu đô la.
* **Kinh tế:** Theo báo cáo của PWC, tội phạm mạng là một trong những loại hình tội phạm kinh tế phổ biến nhất tại Việt Nam. Ước tính thiệt hại do virus máy tính gây ra cho người dùng Việt Nam đã lên tới 20.892 tỷ đồng vào năm 2019. Trên phạm vi toàn cầu, McAfee và CSIS ước tính tội phạm mạng đã gây thiệt hại tới 600 tỷ đô la cho nền kinh tế thế giới vào năm 2017.
* **An ninh quốc gia:** ATANTT tác động trực tiếp đến an ninh và chủ quyền của một quốc gia, thể hiện qua các vụ việc gián điệp mạng như NSA nghe lén các nhà lãnh đạo thế giới, hay các cuộc tấn công có chủ đích vào hệ thống sân bay của Việt Nam.

## II. Các khái niệm và mục tiêu cơ bản

Để xây dựng một hệ thống an toàn, chúng ta cần hiểu và cân bằng ba thành phần:

1.  **Chính sách ATANTT (Security Policy):** Một tuyên bố về các mục tiêu và yêu cầu an toàn của hệ thống, định rõ các hành vi được phép/không được phép của chủ thể trên các tài nguyên.
2.  **Mô hình đe dọa (Threat Model):** Mô tả các mối đe dọa mà kẻ tấn công có thể gây ra cho hệ thống và hậu quả của chúng. Nó trả lời các câu hỏi: Cái gì cần bảo vệ?, Ai có thể tấn công?, và Hệ thống bị tấn công như thế nào?.
3.  **Cơ chế ATANTT (Security Mechanism):** Các kỹ thuật, thủ tục để thực thi và đảm bảo chính sách được tuân thủ.

### 1. Mục tiêu ATANTT

* **Mô hình CIA:**
    * **Bí mật (Confidentiality):** Tài nguyên chỉ được tiếp cận bởi các bên được ủy quyền.
    * **Toàn vẹn (Integrity):** Tài nguyên chỉ được sửa đổi bởi các bên được ủy quyền.
    * **Sẵn sàng (Availability):** Tài nguyên sẵn sàng khi có yêu cầu hợp lệ.
* **Mô hình AAA:**
    * **Đảm bảo (Assurance):** Hệ thống cung cấp và quản trị được sự tin cậy.
    * **Xác thực (Authenticity):** Khẳng định được danh tính của chủ thể trong hệ thống.
    * **Ẩn danh (Anonymity):** Che giấu được thông tin cá nhân của chủ thể.

### 2. Cơ chế ATANTT

Các cơ chế được chia thành hai nhóm chính:

* **Ngăn chặn (Prevention):** Ngăn chặn chính sách bị xâm phạm.
* **Phát hiện (Detection) và Ứng phó (Response):** Phát hiện khi chính sách bị xâm phạm để có hành động đối phó.

Một số cơ chế cụ thể bao gồm: Bảo vệ vật lý, Mật mã học, Định danh, Xác thực, Ủy quyền, Ghi nhật ký (Logging), Kiểm toán (Auditting), Sao lưu và Khôi phục, Dự phòng (Redundancy)... 

## III. Nguyên lý xây dựng hệ thống an toàn

Quá trình xây dựng một hệ thống an toàn gồm 4 giai đoạn: Phân tích yêu cầu, Thiết kế, Triển khai, và Kiểm thử & bảo trì. Quá trình này cần tuân thủ các nguyên tắc cốt lõi sau:

* **An toàn là bài toán kinh tế (Security is Economics):** Phải có sự cân bằng giữa chi phí bảo vệ và giá trị của tài sản cần bảo vệ. Do đó, cần **giữ cho hệ thống đơn giản (Keep It Simple, Sir!)** vì sự phức tạp là kẻ thù của an toàn.
* **Mắt xích yếu nhất (Weakest Link):** An toàn của hệ thống được quyết định bởi thành phần yếu nhất.
* **Bảo vệ theo chiều sâu (Defense in Depth):** Xây dựng nhiều lớp bảo vệ khác nhau cho tài nguyên.
* **Mặc định an toàn (Fail-safe Default):** Khi có lỗi, hệ thống nên mặc định xử lý theo hướng an toàn, ví dụ như từ chối quyền truy cập.
* **Kiểm soát tất cả truy cập (Complete Mediation):** Mọi yêu cầu truy cập tới tài nguyên đều phải được kiểm tra quyền một cách đầy đủ. Cần cảnh giác với lỗ hổng **TOCTTOU (Time Of Check To Time Of Use)**, nơi trạng thái hệ thống có thể bị thay đổi giữa thời điểm kiểm tra và thời điểm sử dụng quyền.
* **Thiết kế mở (Open Design):** An toàn của hệ thống không nên dựa vào việc che giấu thuật toán hay thiết kế. Theo **Nguyên lý Shannon (Shannon's Maxim)**: "Kẻ thù luôn biết hệ thống".
* **Tách biệt các nguyên tắc:**
    * **Tối thiểu hóa quyền (Least Privilege):** Mỗi chủ thể chỉ nên được cấp những quyền hạn tối thiểu cần thiết để hoàn thành công việc.
    * **Phân chia quyền (Privilege Separation):** Chia hệ thống thành các thành phần nhỏ, mỗi thành phần có quyền hạn tối thiểu nhất có thể.
    * **Chia sẻ trách nhiệm (Separation of Responsibility):** Yêu cầu sự chấp thuận từ nhiều bên để thực hiện một hành động quan trọng.
* **Tính dễ sử dụng (Usability):** Các cơ chế an toàn phải dễ sử dụng, nếu không người dùng sẽ tìm cách đi đường vòng, làm giảm an toàn của hệ thống.

## IV. Cơ sở tính toán được tin cậy (Trusted Computing Base - TCB)

* **Định nghĩa:** TCB là một tập hợp con của hệ thống (bao gồm cả phần cứng và phần mềm) mà toàn bộ các mục tiêu an toàn của hệ thống phải dựa vào đó.
* **Yêu cầu với TCB:**
    * Không thể vòng tránh (Unbypassable).
    * Chống sửa đổi (Tamper-resistant).
    * Có thể thẩm tra (Verifiable).
* **Vấn đề "Tin tưởng vào sự tin cậy" (Trusting Trust):** Đây là một vấn đề triết học trong khoa học máy tính do Ken Thompson đặt ra. Khi chúng ta tin tưởng một chương trình, thực chất chúng ta đang tin tưởng trình biên dịch đã tạo ra nó, rồi lại tin vào hệ điều hành, và cứ thế. Điều này cho thấy tầm quan trọng của việc xác định và bảo vệ TCB một cách cẩn thận.
* **Ví dụ thực tế:** Chip TPM (Trusted Platform Module), Apple Secure Enclave Processor, Samsung TEEGRIS... là các ví dụ về việc triển khai TCB trong phần cứng.