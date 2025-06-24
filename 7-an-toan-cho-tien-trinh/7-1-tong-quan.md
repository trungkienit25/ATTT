# Bài 7: An toàn cho tiến trình (Process Security)

Các bài học trước đã tập trung vào các khái niệm, mô hình và giao thức ở mức cao. Bài học này sẽ đi sâu vào các lỗ hổng ở mức độ thấp hơn, liên quan trực tiếp đến cách chương trình được thực thi và quản lý bộ nhớ. Đây là những lỗ hổng phần mềm nguy hiểm và phổ biến nhất, ví dụ như **Tràn bộ đệm (Buffer Overflow)**, vốn luôn nằm trong danh sách các lỗ hổng nguy hiểm nhất hàng năm (ví dụ: 2023 CWE Top 25).

### 1. Tổng quan về tiến trình

Để hiểu các lỗ hổng này, trước tiên chúng ta cần ôn lại cách một tiến trình được tổ chức và thực thi trong bộ nhớ.

#### 1.1. Tiến trình là gì?

* Cần phân biệt giữa **chương trình (program)** và **tiến trình (process)**.
    * **Chương trình** là một tập hợp các chỉ thị nằm trên đĩa cứng (file thực thi).
    * **Tiến trình** là một chương trình đang được thực thi.
* Các tài nguyên tối thiểu của một tiến trình bao gồm:
    * Vùng nhớ được cấp phát.
    * Con trỏ lệnh (Program Counter - PC, hay EIP trong x86) để theo dõi lệnh đang thực thi.
    * Các thanh ghi của CPU.
* Hệ điều hành quản lý thông tin của mỗi tiến trình thông qua một cấu trúc dữ liệu gọi là **Khối điều khiển tiến trình (Process Control Block - PCB)**.

#### 1.2. Cấp phát bộ nhớ cho tiến trình

* **Bộ nhớ ảo (Virtual Memory):** Để cách ly và bảo vệ các tiến trình, hệ điều hành hiện đại sử dụng cơ chế bộ nhớ ảo. Mỗi tiến trình sẽ có một không gian địa chỉ ảo riêng, bắt đầu từ 0. Hệ điều hành và CPU sẽ chịu trách nhiệm ánh xạ các địa chỉ ảo này sang các địa chỉ trên bộ nhớ vật lý (RAM).
* **Lợi ích của bộ nhớ ảo**:
    * **Cách ly:** Hai tiến trình dù truy cập cùng một địa chỉ ảo nhưng sẽ tương ứng với hai ô nhớ vật lý khác nhau.
    * **Tối thiểu hóa quyền:** Hệ điều hành kiểm soát để một tiến trình chỉ có thể truy cập vào không gian bộ nhớ của chính nó.

* **Cấu trúc bộ nhớ của tiến trình (x86 32-bit):** 
    * **Text:** Chứa mã thực thi của chương trình.
    * **Data:** Chứa các biến toàn cục và biến tĩnh đã được khởi tạo.
    * **BSS:** Chứa các biến toàn cục và biến tĩnh chưa được khởi tạo.
    * **Heap:** Vùng nhớ được cấp phát động khi chương trình chạy (ví dụ: thông qua `malloc`). Vùng heap phát triển từ địa chỉ thấp lên địa chỉ cao.
    * **Stack:** Vùng nhớ dùng để lưu trữ các biến cục bộ, tham số của hàm và địa chỉ trả về khi một hàm được gọi. Vùng stack phát triển từ địa chỉ cao xuống địa chỉ thấp.

#### 1.3. Kiến trúc x86 và Lời gọi hàm

Để hiểu về tấn công tràn bộ đệm trên stack, ta cần hiểu cách stack hoạt động trong các lời gọi hàm trên kiến trúc x86.

* **Thanh ghi chính:**
    * `EIP` (Instruction Pointer): Trỏ tới lệnh tiếp theo sẽ được thực thi.
    * `ESP` (Stack Pointer): Luôn trỏ tới đỉnh của stack.
    * `EBP` (Base Pointer): Trỏ tới đáy của **khung stack (stack frame)** hiện tại.
* **Khung stack (Stack Frame):** Là một vùng nhớ trên stack được tạo ra riêng cho mỗi lời gọi hàm để lưu trữ:
    * Các tham số truyền cho hàm.
    * Các biến cục bộ của hàm.
    * Địa chỉ trả về (Return Address - RIP), là địa chỉ của lệnh mà `EIP` sẽ quay về thực thi sau khi hàm kết thúc.
    * Con trỏ khung stack của hàm gọi nó (Saved Frame Pointer - SFP).

* **Quy trình gọi hàm (Calling Convention):** 
    1.  **Hàm gọi (Caller):**
        * Đẩy các tham số vào stack theo thứ tự ngược lại.
        * Thực thi lệnh `call`. Lệnh này sẽ đẩy địa chỉ trả về (RIP) vào stack và nhảy `EIP` đến đầu hàm được gọi (callee).
    2.  **Hàm được gọi (Callee) - Phần mở đầu (Prologue):**
        * Đẩy giá trị `EBP` hiện tại vào stack (để lưu con trỏ khung stack của hàm gọi - SFP).
        * `mov %esp, %ebp`: Thiết lập `EBP` mới cho khung stack của hàm hiện tại.
        * `sub $X, %esp`: Cấp phát `X` byte trên stack cho các biến cục bộ.
    3.  **(Thân hàm thực thi)**
    4.  **Hàm được gọi (Callee) - Phần kết (Epilogue):**
        * `mov %ebp, %esp`: Giải phóng vùng nhớ của các biến cục bộ.
        * `pop %ebp`: Khôi phục `EBP` của hàm gọi từ giá trị SFP đã lưu.
        * `ret`: Lấy địa chỉ trả về (RIP) từ đỉnh stack và gán cho `EIP`, trả quyền điều khiển về cho hàm gọi.
    5.  **Hàm gọi (Caller):** Dọn dẹp các tham số đã đẩy vào stack.