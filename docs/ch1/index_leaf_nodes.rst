=======================
Nút lá trên cây chỉ mục
=======================
 
Mục đích chính của chỉ mục là tạo ra một biểu diễn có trật tự nào đó cho
dữ liệu được đánh chỉ mục. Tuy nhiên, ta không thể lưu trữ dữ liệu một
cách liên tục trên bộ nhớ vật lý vì thao tác chèn từ các câu lệnh
**``insert``** sẽ bắt ta phải di chuyển hàng loạt bản ghi để tạo chỗ
trống cho bản ghi mới được thêm vào. Di chuyển một lượng dữ liệu lớn là
rất chậm chạp, khiến cho câu lệnh **``insert``** trở nên tốn kém thời
gian. Giải pháp cho vấn đề này đó là việc tạo lập ra một thứ tự logic
độc lập hoàn toàn với thứ tự vật lý trên bộ nhớ.

Thứ tự logic này có thể được tạo ra nhờ một danh sách nối đôi. Danh sách
này bao gồm các nút, mỗi nút chứa hai liên kết tới nút kế trước và nút
kế sau nó. Việc chèn một nút mới vào danh sách này rất tiện lợi, ta chỉ
việc cập nhật nút kế trước và nút kế sau trỏ đến nó. Địa chỉ vật lý của
mỗi nút không quan trọng bởi vì danh sách này chỉ duy trì thứ tự logic
mà thôi.

Cấu trúc dữ liệu này gọi là danh sách nối đôi bởi vì mỗi nút tham chiếu
tới hai nút kế trước và kế sau nó. Điều này giúp ta có thể duyệt qua
danh sách theo cả hai chiều xuôi và ngược một cách dễ dàng. Thao tác
thêm dữ liệu mới cũng rất đơn giản vì ta chỉ cần cập nhật một vài con
trỏ.

Danh sách nối đôi cũng là một cấu trúc dữ liệu phổ biến được dùng để cài
đặt các ``collections`` (hay ``containers`` ) trong nhiều ngôn ngữ lập
trình khác nhau:

============== =========================================
Ngôn ngữ       Tên
============== =========================================
Java           ``java.util.LinkedList``
.NET Framework ``System.Collections.Generic.LinkedList``
C++            ``std::list``
============== =========================================

Các cơ sở dữ liệu sử dụng danh sách nối đôi để kết nối các nút lá chỉ
mục. Mỗi nút lá được lưu trữ trong một trang hoặc khối. Một khối là một
đơn vị lưu trữ nhỏ nhất trong cơ sở dữ liệu và có kích thước cố định
khoảng vài kilobytes. Cơ sở dữ liệu sẽ cố gắng thêm một số lượng bản ghi
chỉ mục lớn nhất có thể vào mỗi khối. Như vậy, thứ tự của các bản ghi
được quản lý ở hai mức: thứ tự giữa chúng trong mỗi nút lá và thứ tự của
các nút lá trong danh sách nối đôi.

.. raw:: html

   <p align="center">

.. raw:: html

   <figure style="justify-content: center; text-align: center" class="image">

.. raw:: html

   <figcaption>

Hình 1.1. Nút lá chỉ mục và bảng dữ liệu tương ứng

.. raw:: html

   </figcaption>

.. raw:: html

   </figure>

.. raw:: html

   </p>

**Hình 1.1.** mô phỏng các nút lá chỉ mục và kết nối giữa chúng với dữ
liệu thực tế trong bảng. Mỗi bản ghi chỉ mục bao gồm giá trị khóa tại
cột được đánh chỉ mục (cột 2) và tham chiếu tới địa chỉ của hàng tương
ứng trong bảng (thông qua ``ROWID``). Có thể thấy giá trị khóa của các
bản ghi trong một nút lá cũng như giữa các nút lá với nhau đều được sắp
xếp theo thứ tự tăng dần từ trên xuống dưới. Trái lại, dữ liệu thực trên
bảng được lưu theo một cấu trúc dạng đống và hoàn toàn không được sắp
xếp. Giữa các hàng trong khối và giữa các khối với nhau hoàn toàn không
có một mối quan hệ nào khác.