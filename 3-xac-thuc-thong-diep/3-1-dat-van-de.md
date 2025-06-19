# Bài 3: Xác thực thông điệp

Trong các bài học trước, chúng ta đã tập trung vào **tính bí mật (confidentiality)**, tức là làm thế nào để che giấu nội dung thông tin. Tuy nhiên, trong nhiều trường hợp, việc đảm bảo rằng thông tin không bị thay đổi và đến từ đúng người gửi còn quan trọng hơn. Bài học này sẽ tập trung vào các kỹ thuật để giải quyết vấn đề đó.

### 1. Đặt vấn đề

Hãy xem xét kịch bản giao tiếp cơ bản giữa hai bên, Alice và Bob, qua một kênh truyền tin. Một kẻ tấn công, Mallory, có thể có mặt trên kênh truyền này và thực hiện các hành vi phá hoại:
* Nghe lén thông điệp.
* Chặn và thay đổi nội dung thông điệp M thành M’.
* Giả mạo một thông điệp hoàn toàn mới M’’ và gửi đi dưới danh nghĩa của Alice.

Do đó, Bob, người nhận tin, cần có cách để xác minh thông điệp nhận được.

#### Yêu cầu của Xác thực thông điệp

Một thông điệp được coi là đã được xác thực nếu nó thỏa mãn các yêu cầu sau:

1.  **Nội dung toàn vẹn (Integrity):** Người nhận phải có khả năng kiểm tra được rằng nội dung bản tin không bị sửa đổi trong quá trình truyền. Yêu cầu này bao hàm cả trường hợp chính người nhận (Bob) cố tình sửa đổi nội dung để trục lợi.
2.  **Nguồn gốc tin cậy (Authenticity):** Người nhận phải có khả năng xác minh được rằng bản tin thực sự đến từ người gửi đã khai báo (Alice). Yêu cầu này bao hàm cả việc:
    * Chống lại việc người gửi (Alice) sau này **phủ nhận** đã gửi bản tin đó.
    * Chống lại việc người nhận (Bob) tự tạo ra thông báo rồi **"vu khống"** là do Alice gửi.
3.  **Đúng thời điểm (Timeliness):** Đảm bảo bản tin là mới và phù hợp với phiên giao dịch hiện tại, không phải là một bản tin cũ bị ghi lại và phát lại.

#### Các dạng tấn công điển hình

Từ các yêu cầu trên, chúng ta có thể xác định các dạng tấn công điển hình vào tính xác thực của thông điệp:

1.  **Tấn công thay thế (Substitution Attack):** Kẻ tấn công chặn một thông điệp, thay đổi nội dung của nó rồi chuyển tiếp cho người nhận.
    * **Ví dụ:** Alice gửi yêu cầu chuyển tiền cho ngân hàng: "Tôi là Alice. Số tài khoản của tôi là 456. Hãy chuyển tiền cho tôi!". Kẻ tấn công chặn lại, sửa số tài khoản thành của mình và gửi đi.
2.  **Tấn công giả danh (Masquerade Attack):** Kẻ tấn công mạo danh một bên hợp lệ và khởi tạo các thông điệp để gửi cho bên kia.
    * **Ví dụ:** Kẻ tấn công mạo danh Alice và gửi yêu cầu chuyển tiền tới Bob: "Tôi là Alice. Đây là số tài khoản của tôi. Hãy chuyển tiền cho tôi!".
3.  **Tấn công phủ nhận gửi (Repudiation of Origin):** Bên gửi, sau khi đã gửi một thông tin, lại phủ nhận hành động đó.
    * **Ví dụ:**
        * Alice gửi lệnh cho ngân hàng: "Tôi là Alice. Hãy chuyển tiền của tôi vào tài khoản 123!".
        * Ngân hàng thực hiện và báo lại: "Tôi đã chuyển tiền của cô vào tài khoản 123.".
        * Alice phủ nhận: "Không. Tôi chưa bao giờ yêu cầu chuyển tiền của tôi vào tài khoản 123!".
4.  **Tấn công phát lại (Replay Attack):** Kẻ tấn công ghi lại một thông điệp đã được xác thực từ một phiên trước đó và phát lại nó trong một phiên mới để thu lợi bất chính.