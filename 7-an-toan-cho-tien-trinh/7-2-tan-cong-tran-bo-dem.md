### 2. Tấn công tràn bộ đệm (Buffer Overflow)

Đây là một trong những loại lỗ hổng phần mềm lâu đời và nguy hiểm nhất, thường xuất hiện trong các chương trình được viết bằng ngôn ngữ C/C++.

#### 2.1. Khái niệm

* **Bộ đệm (Buffer):** Là một vùng nhớ liên tiếp được cấp phát để lưu trữ dữ liệu, ví dụ như một mảng các ký tự (xâu).
* **Tràn bộ đệm (Buffer Overflow):** Là tình huống xảy ra khi một chương trình cố gắng ghi dữ liệu vào một bộ đệm nhiều hơn khả năng chứa của nó.
* **Lỗ hổng:** Xuất phát từ việc lập trình viên không kiểm soát chặt chẽ kích thước của dữ liệu đầu vào trước khi sao chép vào bộ đệm.
* **Tấn công:** Kẻ tấn công lợi dụng lỗ hổng này để cố ý gửi một lượng dữ liệu lớn. Phần dữ liệu tràn ra sẽ ghi đè lên các vùng nhớ liền kề quan trọng khác, chẳng hạn như địa chỉ trả về của hàm, làm thay đổi luồng thực thi của tiến trình theo ý muốn của kẻ tấn công.

#### 2.2. Cơ chế hoạt động của tấn công tràn bộ đệm trên Stack

Khi một hàm được gọi, các thông tin quan trọng như biến cục bộ, con trỏ khung stack của hàm gọi (SFP) và đặc biệt là **địa chỉ trả về (Return Instruction Pointer - RIP)** được lưu trên stack.

* Hãy xem xét một hàm có lỗ hổng sau:
    ```c
    void func(char *arg1) {
        char buffer[4];
        strcpy(buffer, arg1); // Hàm strcpy không kiểm tra kích thước
    }
    ```
* Nếu `arg1` là một xâu dài hơn 4 ký tự, ví dụ "AuthMe!", các byte của xâu này sẽ được ghi vào `buffer`. Khi `buffer` đã đầy, các byte tiếp theo sẽ tràn ra và ghi đè lên các ô nhớ liền kề trên stack, bao gồm cả SFP và RIP.
* Khi hàm `func` kết thúc, nó sẽ thực hiện lệnh `ret`, lệnh này sẽ lấy giá trị tại vị trí của RIP trên stack để nhảy đến. Vì vị trí này đã bị kẻ tấn công ghi đè, `EIP` sẽ nhảy đến một địa chỉ do kẻ tấn công kiểm soát, dẫn đến việc thực thi mã độc.

#### 2.3. Khai thác lỗ hổng bằng Code Injection

* **Ý tưởng:** Kẻ tấn công không chỉ ghi đè địa chỉ trả về mà còn chèn cả mã độc (gọi là **shellcode**) vào bộ nhớ, thường là vào chính bộ đệm bị tràn.
* **Quy trình khai thác:**
    1.  Tìm một lỗ hổng tràn bộ đệm trong chương trình.
    2.  Chuẩn bị một chuỗi đầu vào độc hại, bao gồm:
        * **Shellcode:** Một đoạn mã máy thực hiện hành động mong muốn (ví dụ: mở một giao diện dòng lệnh - shell).
        * **Padding:** Các byte đệm để lấp đầy phần còn lại của bộ đệm.
        * **Địa chỉ trả về mới:** Địa chỉ của vùng nhớ chứa shellcode.
    3.  Gửi chuỗi này làm đầu vào cho chương trình.
    4.  Khi hàm có lỗ hổng trả về, thay vì quay lại hàm gọi, nó sẽ nhảy đến và thực thi shellcode của kẻ tấn công.

#### 2.4. Tràn bộ đệm trên Heap và các lỗ hổng khác

* **Tràn bộ đệm trên Heap (Heap Overflow):** Tương tự như trên stack, nhưng xảy ra với các vùng nhớ được cấp phát động trên heap (bằng `malloc` hoặc `new`). Việc tràn bộ đệm trên heap có thể ghi đè lên các đối tượng dữ liệu liền kề, đặc biệt nguy hiểm là có thể ghi đè lên các con trỏ quản lý của heap hoặc con trỏ bảng ảo (vtable) trong C++, từ đó chiếm quyền điều khiển chương trình.
* **Use-after-free:** Một lỗi logic nguy hiểm khác, trong đó chương trình tiếp tục sử dụng một con trỏ sau khi vùng nhớ mà nó trỏ tới đã được giải phóng. Kẻ tấn công có thể yêu cầu cấp phát bộ nhớ mới, được cấp phát lại đúng vùng nhớ vừa giải phóng đó, và ghi mã độc vào. Khi chương trình sử dụng lại con trỏ cũ, nó sẽ thực thi mã của kẻ tấn công.

#### 2.5. Các kỹ thuật phòng chống

1.  **Lập trình an toàn (Secure Coding):** Luôn kiểm tra kích thước dữ liệu đầu vào. Sử dụng các hàm xử lý xâu an toàn hơn, có kiểm tra biên, như `fgets()`, `strncpy()`, `strlcpy()` thay vì `gets()`, `strcpy()`.
2.  **Giá trị canh giữ (Stack Canaries/Stack Guard):** Trình biên dịch sẽ đặt một giá trị ngẫu nhiên (gọi là "canary") trên stack, ngay trước địa chỉ trả về. Trước khi hàm trả về, nó sẽ kiểm tra xem giá trị canary có còn nguyên vẹn không. Nếu giá trị này đã bị thay đổi (do tràn bộ đệm), chương trình sẽ bị dừng lại ngay lập tức để ngăn chặn cuộc tấn công.
3.  **Không cho phép thực thi dữ liệu (Data Execution Prevention - DEP / Non-executable Pages):** Hệ điều hành đánh dấu các vùng nhớ chứa dữ liệu như stack và heap là không được phép thực thi. Kỹ thuật này ngăn chặn việc thực thi shellcode được tiêm vào các vùng này.
    * **Hạn chế:** Kẻ tấn công có thể vượt qua bằng các kỹ thuật **Return-Oriented Programming (ROP)**, trong đó chúng không tiêm mã mới mà lợi dụng và móc nối các đoạn mã đã có sẵn trong bộ nhớ (như các hàm thư viện) để thực hiện hành vi độc hại.
4.  **Ngẫu nhiên hóa Bố cục Không gian Địa chỉ (Address Space Layout Randomization - ASLR):** Hệ điều hành sẽ sắp xếp các vùng nhớ (stack, heap, thư viện) vào các địa chỉ ngẫu nhiên mỗi khi chương trình được chạy. Điều này làm cho kẻ tấn công rất khó đoán được địa chỉ của shellcode hoặc các đoạn mã cần thiết cho tấn công ROP.
5.  **Mã xác thực con trỏ (Pointer Authentication Code - PAC):** Là một cơ chế bảo vệ phần cứng (ví dụ: trên chip ARM) sử dụng các bit không dùng đến trong một con trỏ 64-bit để lưu một chữ ký mật mã của con trỏ đó. Trước khi sử dụng một con trỏ quan trọng (như địa chỉ trả về), CPU sẽ kiểm tra chữ ký này. Nếu con trỏ đã bị sửa đổi, chữ ký sẽ không hợp lệ và cuộc tấn công bị ngăn chặn.
6.  **Bảo vệ theo chiều sâu:** Kết hợp nhiều kỹ thuật phòng chống (ví dụ: ASLR + DEP + Stack Canaries) sẽ khiến việc khai thác lỗ hổng trở nên khó khăn hơn rất nhiều.