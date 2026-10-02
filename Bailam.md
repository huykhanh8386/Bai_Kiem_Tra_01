Câu 1: 
- Value Type lưu trực tiếp giá trị của biến, thường liên quan đến vùng nhớ Stack. Khi gán sang biến khác thì dữ liệu được sao chép độc lập.
- Reference Type lưu địa chỉ tham chiếu đến đối tượng, còn đối tượng thường nằm trên Heap. Khi gán sang biến khác, hai biến có thể cùng trỏ đến một đối tượng
- Ví dụ Value Type: int, double, bool, struct. Reference Type: class, array, string

Câu 2: 
- Thuộc tính có set có thể thay đổi giá trị nhiều lần sau khi đối tượng được tạo.
- Thuộc tính có init chỉ được gán khi khởi tạo đối tượng, sau đó không thể thay đổi tùy ý.
- Thường dùng cho các dữ liệu cần cố định như mã nhân viên, mã sản phẩm, mã hóa đơn.

Câu 3: 
- virtual được khai báo ở lớp cha để cho phép lớp con ghi đè phương thức.
- override được dùng ở lớp con để viết lại cách hoạt động của phương thức đó.
- Nhờ virtual và override, chương trình có thể thực hiện tính đa hình và tự gọi đúng phương thức của lớp con khi chạy.

Câu 4: 
- Thành phần static thuộc về lớp, không thuộc riêng một đối tượng.
- Nó chỉ có một bản dùng chung cho tất cả các đối tượng của lớp.
- Vì vậy phải truy cập thông qua tên lớp, không thông qua object được tạo bằng new.
- static thường dùng cho biến đếm, dữ liệu dùng chung hoặc các phương thức tiện ích.
