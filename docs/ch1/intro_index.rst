=====================
Introduction to index
=====================

“Chỉ mục giúp truy vấn nhanh hơn” - đây là lời giải thích đơn giản nhất
có thể về chỉ mục. Mặc dù nhận định này đề cập đến khía cạnh quan trọng
nhất của chỉ mục đó là tốc độ, nó vẫn chưa phải là tất cả những gì sẽ
được bàn đến trong cuốn sách này. Chương đầu tiên sẽ đề cập tới cấu trúc
chỉ mục một cách khá cụ thể nhưng không quá đi sâu vào chi tiết, với mục
đích chính là cung cấp một cái nhìn vừa đủ cho người đọc để có thể hiểu
về các khía cạnh của hiệu năng SQL được đề cập xuyên suốt cuốn sách.

Chỉ mục là một cấu trúc đặc biệt trong cơ sở dữ liệu và được xây dựng
qua câu lệnh create index. Nó cần có một vùng nhớ riêng để lưu trữ những
dữ liệu trong bảng mà được đánh chỉ mục. Nói cách khác, chỉ mục hoàn
toàn mang tính chất dư thừa, tạo ra chỉ mục không làm thay đổi gì tới dữ
liệu của bảng mà chỉ khởi tạo một cấu trúc mới tham chiếu đến bảng đã có
mà thôi. Chỉ mục cơ sở dữ liệu nhìn chung rất giống với mục lục nằm ở
cuối cuốn sách: chúng đều chiếm không gian riêng, đều dư thừa và đều
tham chiếu tới thông tin thực tế nằm tại một vị trí khác.

   **CHỈ MỤC PHÂN CỤM**

   SQL Server và MySQL (sử dụng InnoDB) có cái nhìn rộng hơn về ý nghĩa
   của chỉ mục. Các hệ thống này chứa những bảng đặc biệt chỉ bao gồm
   các cấu trúc chỉ mục và gọi chúng là những cụm chỉ mục. Oracle lại
   gọi những bảng này với tên gọi là “bảng tổ chức chỉ mục”
   (Index-Organized Tables - IOT).

   Tại chương 5 - “Phân cụm dữ liệu”, ta sẽ tìm hiểu kĩ hơn về khái niệm
   này, đồng thời lý giải những lợi thế và hạn chế của nó.

Quá trình tìm kiếm trên chỉ mục cơ sở dữ liệu tương đối giống với tìm
kiếm số điện thoại trên quyển sổ danh bạ. Điều kiện tiên quyết chính là
các bản ghi phải được sắp xếp theo một thứ tự xác định. Tìm kiếm trên
một cấu trúc dữ liệu đã được sắp xếp rất nhanh chóng và đơn giản vì thứ
tự của một bản ghi chính là vị trí của nó.

Tuy nhiên, chỉ mục trên cơ sở dữ liệu phức tạp hơn một cuốn danh bạ
thông thường ở chỗ dữ liệu của nó liên tục thay đổi. Việc cập nhật cuốn
danh bạ điện thoại ngay khi có sự thay đổi xảy ra là bất khả thi vì
không có không gian trống nào được dành ra giữa hai dòng kế tiếp để chèn
thông tin mới vào. Bởi vậy, những sự thay đổi này chỉ được thực hiện một
cách định kỳ trong những lần tái bản danh bạ tiếp theo. Cơ sở dữ liệu
SQL thì không thể chờ lâu như vậy, nó phải thực hiện các câu lệnh thêm,
sửa, xóa dữ liệu ngay lập tức, đồng thời đảm bảo giữ đúng thứ tự chỉ mục
mà không phải di chuyển cả khối dữ liệu lớn.

Người ta giải quyết vấn đề này bằng cách sử dụng kết hợp hai cấu trúc dữ
liệu cơ bản là danh sách nối đôi và cây tìm kiếm. Hai cấu trúc này cũng
lý giải phần lớn các đặc tính của hiệu năng cơ sở dữ liệu.