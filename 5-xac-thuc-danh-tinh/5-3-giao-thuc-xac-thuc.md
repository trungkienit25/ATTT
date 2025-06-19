### 3. Một số giao thức xác thực

Sau khi tìm hiểu về các phương pháp và vấn đề của việc xác thực bằng mật khẩu, chúng ta sẽ xem xét một số giao thức cụ thể được thiết kế để thực hiện việc xác thực một cách an toàn hơn.

#### 3.1. Giao thức PAP (Password Authentication Protocol)

* Đây là một giao thức xác thực rất cơ bản, trước đây từng được sử dụng trong giao thức mạng PPP.
* **Quy trình:**
    1.  Bên cần xác thực (User - U) gửi định danh và mật khẩu ở dạng **rõ** (không mã hóa) cho bên thẩm tra (Server - S): `U → S: ID || Password`.
    2.  Máy chủ kiểm tra thông tin trong cơ sở dữ liệu và gửi lại thông báo chấp nhận (ACK) hoặc từ chối (NAK).
* **Đánh giá:** Giao thức này **không an toàn** vì mật khẩu được truyền đi ở dạng rõ, có thể dễ dàng bị nghe lén.

#### 3.2. Giao thức CHAP (Challenge Handshake Authentication Protocol)

CHAP là một cải tiến so với PAP, sử dụng cơ chế **thách thức-phản hồi (challenge-response)** để tránh việc truyền mật khẩu ở dạng rõ.

* **Quy trình:**
    1.  U → S: `Request` (Yêu cầu xác thực).
    2.  S → U: `Challenge` (Máy chủ gửi lại một chuỗi ký tự ngẫu nhiên, gọi là "thách thức").
    3.  U → S: `ID || Hash(ID || Hash(Password) || Challenge)` (Người dùng không gửi mật khẩu, mà gửi lại kết quả của một hàm băm, thường là MD5, kết hợp giữa định danh, mật khẩu đã băm và chuỗi thách thức).
    4.  Máy chủ tự thực hiện lại phép tính băm tương tự để kiểm tra kết quả. Nếu trùng khớp, xác thực thành công.
* **Đánh giá:** An toàn hơn PAP vì mật khẩu không bị lộ trên đường truyền. Giá trị phản hồi sẽ luôn khác nhau trong mỗi phiên do giá trị `Challenge` là ngẫu nhiên.

#### 3.3. Giao thức EAP (Extensible Authentication Protocol)

* EAP không phải là một giao thức cụ thể mà là một **khuôn khổ xác thực (framework)** cho phép sử dụng nhiều cơ chế xác thực khác nhau.
* Có khoảng 40 biến thể của EAP, kết hợp với nhiều cơ chế khác nhau:
    * **EAP-MD5:** Tương tự như CHAP.
    * **EAP-TLS, EAP-TTLS, PEAP:** Kết hợp với giao thức TLS để tạo kênh an toàn và xác thực bằng chứng thư số.
    * **EAP-POTP:** Kết hợp với Mật khẩu dùng một lần (One-Time-Password).
    * **EAP-PSK:** Kết hợp với khóa chia sẻ trước (Pre-Shared Key).

#### 3.4. Các giao thức xác thực hai chiều

Trong nhiều trường hợp, không chỉ máy khách cần xác thực với máy chủ mà máy chủ cũng cần xác thực với máy khách để chống lại các tấn công giả mạo máy chủ. Đây được gọi là **xác thực hai chiều (mutual authentication)**.

* **Xác thực 2 chiều dùng Khóa đối xứng (KĐX):**
    * Giả sử A và B đã chia sẻ trước khóa `KS`.
    * **Quy trình:**
        1.  A → B: `IDA` (A tự giới thiệu).
        2.  B → A: `NB` (B thách thức A bằng một nonce `NB`).
        3.  A → B: `f(KS, NB) || NA` (A trả lời thách thức của B và gửi lại thách thức `NA` của mình).
        4.  B → A: `f(KS, NA)` (B trả lời thách thức của A).
    * Hàm `f` có thể là hàm mã hóa hoặc hàm băm.

* **Xác thực 2 chiều dùng Khóa công khai (KCK) (chuẩn ISO/IEC 9798-3):**
    * Sử dụng chứng thư số và chữ ký số để xác thực lẫn nhau.
    * **Quy trình:**
        1.  A → B: `Request`.
        2.  B → A: `TokenID || NB` (B gửi thách thức `NB`).
        3.  A → B: `TokenID || CertA || TokenAB` (A gửi chứng thư của mình và một token `TokenAB` chứa chữ ký số của A lên các nonce để xác thực với B).
        4.  B → A: `TokenID || CertB || TokenBA` (Sau khi xác thực A, B gửi lại chứng thư và token `TokenBA` chứa chữ ký của B để xác thực với A).