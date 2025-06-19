### 3. Các giao thức phân phối khóa công khai (Public Key Distribution Protocols)

Mật mã khóa công khai giải quyết được vấn đề trao đổi khóa bí mật, nhưng nó lại làm nảy sinh một vấn đề mới: **Làm thế nào để phân phối và xác thực các khóa công khai?**  Làm sao Alice có thể chắc chắn rằng khóa công khai mà cô nhận được đúng là của Bob, chứ không phải của một kẻ tấn công? 

#### 3.1. Phân phối khóa không tập trung

Trong mô hình này, người dùng tự trao đổi và xác thực khóa công khai của nhau.

* **Vấn đề:** Các giao thức trao đổi khóa công khai trực tiếp rất dễ bị **tấn công Người đứng giữa (Man-in-the-Middle)**. Kẻ tấn công (C) có thể xen vào giữa Alice và Bob, chặn khóa công khai của mỗi người và gửi đi khóa công khai giả mạo của mình. Kết quả là C có thể đọc và sửa đổi toàn bộ thông tin mà không bị phát hiện.
* **Giải pháp - Mạng lưới tin cậy (Web of Trust):** Một người dùng có thể dựa vào sự tin cậy của các bên trung gian để xác thực khóa. Ví dụ, nếu A đã trao đổi khóa an toàn với C, và C đã trao đổi khóa an toàn với B, thì A có thể tin tưởng vào khóa của B thông qua sự xác thực của C. Tuy nhiên, mô hình này phức tạp và khó quản lý trên quy mô lớn.

#### 3.2. Phân phối khóa tập trung và Hạ tầng khóa công khai (PKI)

Giải pháp phổ biến và có khả năng mở rộng nhất là sử dụng một **bên thứ ba tin cậy (Trusted Third Party)**.

* **PKA (Public Key Authority):** Một mô hình ban đầu, trong đó có một máy chủ trung tâm (PKA) lưu trữ khóa công khai của tất cả người dùng. Khi A muốn nói chuyện với B, A phải hỏi PKA để lấy khóa công khai của B. Hạn chế của mô hình này là PKA phải luôn trực tuyến và có thể trở thành nút cổ chai.
* **CA (Certificate Authority) và Chứng thư số:** Để khắc phục nhược điểm trên, một mô hình tiên tiến hơn ra đời. Bên thứ ba tin cậy, lúc này gọi là **Nhà cung cấp chứng thực (CA)**, sẽ không chỉ lưu trữ mà còn cấp phát một **chứng thư số (Digital Certificate)** cho mỗi người dùng.
    * **Chứng thư số:** Là một văn bản điện tử được ký bởi CA, có vai trò gắn kết định danh của một người dùng với khóa công khai của người đó. Ví dụ: `Cert_A = Chữ_ký_của_CA(ID_A || kU_A || Thời_hạn)`.
    * Với chứng thư số, người dùng có thể trao đổi khóa công khai của nhau một cách ngoại tuyến. Alice chỉ cần gửi chứng thư số của mình cho Bob. Bob, vì đã tin tưởng vào CA, có thể dùng khóa công khai của CA để kiểm tra chữ ký trên chứng thư và từ đó tin tưởng vào khóa công khai của Alice.

#### 3.3. Hạ tầng khóa công khai (Public Key Infrastructure - PKI)

PKI là một hệ thống toàn diện bao gồm phần cứng, phần mềm, chính sách, và các thủ tục cần thiết để tạo, quản lý, phân phối, sử dụng, lưu trữ và thu hồi các chứng thư số.

* **Các thành phần của PKI**:
    * **RA (Registration Authority):** Cơ quan đăng ký, có trách nhiệm xác thực danh tính của người dùng trước khi CA cấp chứng thư.
    * **CA (Certification Authority):** Nhà cung cấp chứng thực, chịu trách nhiệm phát hành, gia hạn và thu hồi chứng thư số.
    * **CR (Certificate Repository):** Kho lưu trữ chứng thư, là nơi công bố các chứng thư số và danh sách thu hồi.
    * **EE (End-Entity):** Đối tượng cuối, là người dùng hoặc thiết bị sử dụng chứng thư số.

* **Chứng thư số X.509:** Là tiêu chuẩn phổ biến nhất cho chứng thư số, bao gồm các trường thông tin quan trọng như:
    * **Issuer:** Thông tin về CA đã cấp chứng thư.
    * **Subject:** Thông tin về người/tổ chức được cấp chứng thư.
    * **Subject's Public Key:** Khóa công khai của chủ thể.
    * **Validity:** Thời gian hiệu lực của chứng thư (từ ngày... đến ngày...).
    * **Signature:** Chữ ký số của CA trên toàn bộ nội dung chứng thư.

* **Xác thực và Thu hồi chứng thư số:**
    * Khi nhận một chứng thư, cần phải **xác thực** nó bằng cách kiểm tra chữ ký của CA, thời hạn hiệu lực, và quan trọng nhất là **trạng thái thu hồi**.
    * Một chứng thư có thể bị **thu hồi (revoke)** trước khi hết hạn nếu khóa cá nhân tương ứng bị lộ.
    * Có hai cơ chế chính để kiểm tra trạng thái thu hồi:
        1.  **CRL (Certificate Revocation List):** CA định kỳ công bố một danh sách chứa số serial của các chứng thư đã bị thu hồi.
        2.  **OCSP (Online Certificate Status Protocol):** Một dịch vụ cho phép hỏi trực tuyến về trạng thái của một chứng thư cụ thể, cung cấp thông tin cập nhật hơn CRL.

* **Kiến trúc PKI và Chuỗi xác thực (Chain of Trust):**
    * Trong thực tế, các hệ thống PKI thường có **kiến trúc phân cấp (Hierarchical PKI)**, với một **Root CA** ở trên cùng. Root CA ký chứng thư cho các CA cấp dưới (Intermediate CAs), và các CA này lại ký cho người dùng cuối hoặc các CA cấp thấp hơn nữa.
    * Để tin tưởng một chứng thư của người dùng cuối, máy tính của bạn phải xác thực một **chuỗi chứng thư (chain of trust)**, đi từ chứng thư của người dùng, qua các CA trung gian, và cuối cùng đến một Root CA mà hệ điều hành của bạn đã tin tưởng sẵn. Nếu bất kỳ mắt xích nào trong chuỗi này không hợp lệ, toàn bộ chuỗi sẽ bị từ chối.

#### 3.4. Kết luận

* Việc quản lý và phân phối khóa là một phần cực kỳ quan trọng, quyết định sự an toàn của cả hệ thống. Một hệ mật mã tốt sẽ trở nên vô nghĩa nếu giao thức phân phối khóa bị lỗi.
* Hãy luôn ưu tiên sử dụng các giao thức đã được tiêu chuẩn hóa và kiểm chứng rộng rãi như **IPSec, TLS, IEEE 802.11x, Kerberos,...**.
* Không bao giờ sử dụng cùng một khóa cho nhiều mục đích khác nhau (ví dụ: vừa mã hóa vừa xác thực) hoặc cho hai chiều truyền tin khác nhau.