### 4. Chữ ký số (Digital Signature)

Chữ ký số là một cơ chế mật mã mạnh mẽ, vượt qua những hạn chế của MAC để cung cấp một mức độ đảm bảo cao hơn về nguồn gốc và tính toàn vẹn của thông điệp.

#### 4.1. Khái niệm và Yêu cầu

* **Định nghĩa:** Chữ ký số là một đoạn dữ liệu điện tử được bên gửi gắn vào một văn bản để chứng thực nguồn gốc và nội dung của văn bản đó. Nó tương đương với chữ ký tay trong thế giới thực.
* **Yêu cầu đối với Chữ ký số:** 
    * **Tính xác thực (Authenticity):** Người nhận có thể chứng minh được văn bản được ký bởi đúng người gửi.
    * **Tính toàn vẹn (Integrity):** Người nhận có thể chứng minh được văn bản không bị sửa đổi sau khi ký.
    * **Chống từ chối (Non-repudiation):** Người gửi không thể phủ nhận hành động ký vào văn bản của mình. Đây là điểm khác biệt cốt lõi và quan trọng nhất so với MAC.
    * **Không thể giả mạo (Unforgeable):** Không ai có thể giả mạo chữ ký của người khác.
    * **Không thể tái sử dụng (Non-reusable):** Chữ ký chỉ có giá trị trên một văn bản duy nhất.

#### 4.2. Sơ đồ hoạt động

Một hệ thống chữ ký số thường được xây dựng dựa trên mật mã khóa công khai và bao gồm ba hàm chính: 
1.  **Hàm sinh khóa `Gen()`:** Tạo ra một cặp khóa gồm khóa công khai `pk` (public key) và khóa cá nhân `sk` (secret key).
2.  **Hàm ký `S(sk, m)`:** Sử dụng **khóa cá nhân `sk`** để tạo ra chữ ký `sig` cho thông điệp `m`.
3.  **Hàm kiểm tra `V(pk, m, sig)`:** Sử dụng **khóa công khai `pk`** tương ứng để kiểm tra tính hợp lệ của chữ ký `sig` trên thông điệp `m`.

* **Nguyên tắc:** Bất kỳ ai có khóa công khai `pk` đều có thể kiểm tra chữ ký, nhưng chỉ người nắm giữ khóa cá nhân `sk` mới có thể tạo ra chữ ký.
* **Tính đúng đắn:** `V(pk, m, S(sk, m))` phải luôn trả về `True`.

#### 4.3. Quy trình ký và xác thực bằng mật mã khóa công khai

Để tối ưu hiệu năng (vì ký trên toàn bộ văn bản dài sẽ rất chậm), quy trình thực tế thường kết hợp với hàm băm: 

* **Quy trình ký (Phía người gửi):** 
    1.  Sử dụng một hàm băm (ví dụ: SHA-256) để tính ra một bản tóm lược (digest) `h` của văn bản gốc `m`.
    2.  Dùng **khóa cá nhân** của mình để mã hóa giá trị băm `h`. Kết quả chính là chữ ký số `sig`.
    3.  Gắn chữ ký số `sig` vào văn bản gốc.

* **Quy trình xác thực (Phía người nhận):** 
    1.  Tách chữ ký số `sig` và văn bản `m` ra.
    2.  Sử dụng cùng một hàm băm để tính lại giá trị băm `h` của văn bản `m` nhận được.
    3.  Dùng **khóa công khai** của người gửi để giải mã chữ ký `sig`, thu được giá trị băm `h'`.
    4.  So sánh `h` và `h'`. Nếu chúng giống hệt nhau, chữ ký được xem là hợp lệ, đồng nghĩa với việc văn bản là toàn vẹn và thực sự đến từ người gửi.

#### 4.4. Một số ứng dụng

Chữ ký số có rất nhiều ứng dụng quan trọng trong thực tế: 
* **Ký văn bản điện tử:** Sử dụng trong các hệ thống văn phòng điện tử (doffice), giao dịch công trực tuyến (khai thuế, đăng ký kinh doanh).
* **Xác thực phần mềm:** Các nhà phát hành phần mềm ký lên sản phẩm của họ. Khi người dùng cài đặt, hệ điều hành có thể kiểm tra chữ ký để đảm bảo phần mềm không bị giả mạo hay nhiễm mã độc.
* **Xác thực giao dịch tài chính, ngân hàng**.
* **Xác thực email (DKIM)** để chống giả mạo email.
* **Xác thực đăng nhập** vào các hệ thống quan trọng.

#### 4.5. Chữ ký mù (Blind Signature)

* **Bối cảnh:** Trong một số ứng dụng như bầu cử điện tử hay tiền điện tử, cần đảm bảo tính ẩn danh. Người ký (ví dụ: cơ quan bầu cử) cần xác thực một phiếu bầu nhưng không được biết nội dung của phiếu đó.
* **Định nghĩa:** Chữ ký mù là một dạng chữ ký số mà trong đó, người ký không biết nội dung của văn bản mình đang ký.
* **Quy trình (Ví dụ với bầu cử):** 
    1.  Cử tri (Alice) chuẩn bị lá phiếu của mình `x` và "làm mù" nó bằng một hệ số ngẫu nhiên `r`.
    2.  Alice gửi lá phiếu đã được làm mù cho Cơ quan Bầu cử.
    3.  Cơ quan Bầu cử ký lên lá phiếu mù này và gửi lại cho Alice.
    4.  Alice dùng hệ số ngẫu nhiên `r` để "xóa mù" chữ ký, thu được một chữ ký hợp lệ của Cơ quan Bầu cử trên lá phiếu gốc `x` của mình.
    5.  Khi nộp phiếu, không ai (kể cả Cơ quan Bầu cử) có thể liên kết lá phiếu đã được ký với danh tính của Alice.

#### 4.6. Các vấn đề an toàn cho Chữ ký số

Sự an toàn của chữ ký số phụ thuộc vào hai yếu tố chính: 

1.  **An toàn của khóa cá nhân:** Khóa cá nhân phải được bảo vệ tuyệt đối. Nếu bị đánh cắp, kẻ tấn công có thể giả mạo chữ ký của chủ sở hữu.
    * **Giải pháp:** Bảo vệ khóa bằng mật khẩu, hoặc tốt hơn là lưu trữ trong các thiết bị phần cứng chuyên dụng như **Smart Card** hoặc **USB Token**. Các thiết bị này được thiết kế để quá trình ký diễn ra bên trong chip và khóa cá nhân không bao giờ rời khỏi thiết bị.
2.  **Sự tin cậy của khóa công khai:** Làm thế nào để chắc chắn rằng khóa công khai mình đang dùng để xác thực đúng là của người cần giao dịch, chứ không phải của kẻ tấn công? 
    * **Vấn đề:** Tấn công Man-in-the-Middle, kẻ tấn công tráo khóa công khai giả mạo của mình vào.
    * **Giải pháp:** Sử dụng **Hạ tầng khóa công khai (Public Key Infrastructure - PKI)**. Trong mô hình này, một tổ chức tin cậy (gọi là **Nhà cung cấp chứng thực - CA**) sẽ phát hành các **chứng thư số (Digital Certificate)** để xác nhận mối liên kết giữa một danh tính và một khóa công khai.