### 3.3. Hạ tầng khóa công khai (Public Key Infrastructure - PKI)

Để giải quyết vấn đề làm thế nào để tin tưởng và phân phối khóa công khai một cách an toàn và có hệ thống, **Hạ tầng khóa công khai (PKI)** đã được phát triển.  Đây là giải pháp tiêu chuẩn và được sử dụng rộng rãi nhất hiện nay.

#### Định nghĩa

PKI là một hệ thống toàn diện bao gồm **phần cứng, phần mềm, chính sách, và các thủ tục** cần thiết để tạo, quản lý, phân phối, sử dụng, lưu trữ và thu hồi các chứng thư số.  Mục tiêu cốt lõi của PKI là cung cấp một cơ chế tin cậy để xác thực danh tính và khóa công khai của các thực thể trên mạng. 

#### Các thành phần của PKI

Một hệ thống PKI điển hình bao gồm các thành phần chính sau: 

* **Nhà cung cấp chứng thực (CA - Certification Authority):** Đây là thành phần trung tâm và được tin cậy nhất của PKI.  CA chịu trách nhiệm phát hành, quản lý, gia hạn và thu hồi chứng thư số. 
* **Cơ quan đăng ký (RA - Registration Authority):** Là đơn vị chịu trách nhiệm xác thực danh tính của các đối tượng (người dùng, tổ chức) trước khi họ được CA cấp chứng thư số.  RA hoạt động như một "cánh tay nối dài" của CA. 
* **Kho lưu trữ chứng thư (CR - Certificate Repository):** Là một cơ sở dữ liệu hoặc thư mục dùng để lưu trữ và công bố các chứng thư số cũng như các danh sách thu hồi (CRLs) để các bên có thể truy xuất và kiểm tra. 
* **Đối tượng cuối (EE - End-Entity):** Là người dùng, thiết bị, hoặc ứng dụng, chủ thể của chứng thư số. 

#### Chứng thư số X.509

Chứng thư số là một văn bản điện tử được CA ký để xác thực mối liên kết giữa một danh tính và một khóa công khai.  Tiêu chuẩn phổ biến nhất cho chứng thư số là **X.509**.  Một chứng thư X.509 chứa các thông tin quan trọng sau: 

* **Version:** Phiên bản của chuẩn X.509. 
* **Serial Number:** Số serial duy nhất do CA cấp cho chứng thư. 
* **Signature Algorithm:** Thuật toán mà CA dùng để ký lên chứng thư. 
* **Issuer:** Thông tin định danh của CA đã phát hành chứng thư (Tên tổ chức, quốc gia,...). 
* **Validity:** Khoảng thời gian chứng thư có hiệu lực, bao gồm ngày bắt đầu (`Not Before`) và ngày hết hạn (`Not After`). 
* **Subject:** Thông tin định danh của đối tượng sở hữu chứng thư. 
* **Subject's Public Key Information:** Chứa khóa công khai của đối tượng và thuật toán liên quan. 
* **Extensions:** Các trường thông tin mở rộng khác. 
* **Signature:** Chữ ký số của CA trên toàn bộ nội dung của chứng thư, đảm bảo tính toàn vẹn và xác thực cho chứng thư. 

#### Hoạt động của PKI

1.  **Xác thực chứng thư số (Certificate Validation)**
    Khi nhận được một chứng thư, một ứng dụng hoặc người dùng cần thực hiện một chuỗi các bước kiểm tra để đảm bảo tính tin cậy của nó: 
    * Kiểm tra chữ ký trên chứng thư có hợp lệ không bằng cách sử dụng khóa công khai của CA. 
    * Kiểm tra xem tên thực thể sử dụng có khớp với tên trong trường `Subject` của chứng thư không. 
    * Kiểm tra xem chứng thư có còn trong thời hạn hiệu lực không. 
    * Kiểm tra xem chứng thư có bị thu hồi hay không. 
    * Kiểm tra xem CA phát hành chứng thư có phải là một CA được tin cậy không. 

2.  **Thu hồi chứng thư số (Certificate Revocation)**
    Một chứng thư cần bị thu hồi trước khi hết hạn nếu khóa cá nhân tương ứng bị lộ hoặc thông tin trên chứng thư không còn chính xác.  Có hai cơ chế chính để kiểm tra trạng thái thu hồi: 
    * **CRL (Certificate Revocation List):** CA định kỳ công bố một danh sách được ký, chứa số serial của tất cả các chứng thư đã bị thu hồi.  Hạn chế của CRL là kích thước có thể lớn và thông tin không được cập nhật tức thời. 
    * **OCSP (Online Certificate Status Protocol):** Cung cấp một dịch vụ trực tuyến cho phép kiểm tra trạng thái của một chứng thư cụ thể tại thời điểm truy vấn, khắc phục nhược điểm của CRL. 

#### Kiến trúc PKI và Chuỗi xác thực (Chain of Trust)

* **Kiến trúc phân cấp (Hierarchical PKI):** Trong thực tế, các hệ thống PKI thường được tổ chức theo mô hình phân cấp dạng cây. 
    * Ở trên cùng là **Root CA**, là nguồn gốc của mọi sự tin cậy. Root CA tự ký chứng thư cho chính mình và chứng thư này thường được cài đặt sẵn trong các hệ điều hành và trình duyệt. 
    * Root CA ký chứng thư cho các **CA trung gian (Intermediate CAs)**.
    * Các CA trung gian ký chứng thư cho các đối tượng cuối hoặc các CA cấp thấp hơn. 
* **Chuỗi xác thực (Chain of Trust):** Để một chứng thư của đối tượng cuối được tin cậy, ứng dụng phải có khả năng xác thực một chuỗi liên tục từ chứng thư đó, qua các CA trung gian, và cuối cùng đến một Root CA mà nó tin tưởng.  Nếu bất kỳ chữ ký nào trong chuỗi này không hợp lệ, toàn bộ chuỗi sẽ bị từ chối.