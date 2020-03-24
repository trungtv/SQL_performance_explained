=====================
Cây tìm kiếm (B-tree)
=====================

Các nút lá được lưu trữ theo một thứ tự bất kỳ trên ổ đĩa, hoàn toàn
không liên quan gì tới thứ tự logic giữa chúng. Nó giống như một cuốn
danh bạ mà các trang bị đảo lộn hết thứ tự, nếu bạn đang ở trang chứa
tên “Robinson” và muốn tìm tới liên hệ của “Smith”, bạn sẽ không biết
nên lật trang theo hướng nào để đi tới đích. Cơ sở dữ liệu cần một cấu
trúc khác để tìm đến các nút lá một cách nhanh chóng, đó là cây tìm kiếm
cân bằng (viết tắt: B-tree).

.. raw:: html

   <p align="center">

.. raw:: html

   <figure>

.. raw:: html

   <figcaption>

Hình 1.2. Cấu trúc cây tìm kiếm cân bằng

.. raw:: html

   </figcaption>

.. raw:: html

   </figure>

.. raw:: html

   </p>

**Hình 1.2.** thể hiện một ví dụ về một hệ thống chỉ mục chứa 30 bản ghi
phân thành 10 khối, mỗi khối chứa 03 bản ghi. B-tree gồm 01 nút gốc và
nhiều nút cành giúp tìm kiếm một bản ghi trên nút lá một cách nhanh
chóng hơn.

Mỗi bản ghi trên một nút cành tương ứng với giá trị khóa lớn nhất được
lưu trữ trong khối mà nó quản lý. Như vậy mỗi nút cành ở mức thấp nhất
quản lý cùng lúc nhiều nút lá khác nhau. Các nút cành cao hơn lại quản
lý nút cành ở mức thấp hơn, cây B-tree tiếp tục được xây dựng theo
nguyên tắc này cho đến khi hội tụ tại một nút gốc duy nhất. Cây này gọi
là “cân bằng” bởi vì khoảng cách từ nút gốc tới mọi nút lá là bằng nhau

   **CHÚ Ý:**

   B-tree nghĩa là cây cân bằng (balanced tree), không phải cây nhị phân
   (binary tree).

   Sau khi khởi tạo, cơ sở dữ liệu sẽ duy trì chỉ mục một cách tự động.
   Nó sẽ thực thi tất cả các hành động thêm, sửa, xóa trên index và giữ
   cây chỉ mục ở trạng thái cân bằng. Việc làm này thực hiện một lượng
   lớn thao tác write trên bộ nhớ lưu trữ chỉ mục, tạo nên sự quá tải
   cho việc duy trì.

.. raw:: html

   <p align="center">

.. raw:: html

   <figure>

.. raw:: html

   <figcaption>

Hình 1.3. Duyệt cây tìm kiếm cân bằng

.. raw:: html

   </figcaption>

.. raw:: html

   </figure>

.. raw:: html

   </p>

**Hình 1.3.** thể hiện một nhánh trên chỉ mục nơi xảy ra thao tác duyệt
cây để tìm kiếm bản ghi tương ứng với khóa “57”. Quá trình duyệt bắt đầu
từ nút gốc bên tay trái. Tại nút gốc, các bản ghi sẽ được duyệt qua theo
thứ tự tăng dần giá trị khóa cho đến khi gặp một bản ghi mà tại đó giá
trị khóa lớn hơn hoặc bằng giá trị cần tìm (57). Bản ghi tìm được có giá
trị khóa là 83. Cơ sở dữ liệu sẽ lần theo tham chiếu trên bản ghi tìm
được tới nút cành ở mức thấp hơn. Tại đây, thao tác duyệt được lặp lại,
tìm được bản ghi có giá trị khóa là 57 và tham chiếu tới nút lá chứa bản
ghi cần tìm.

   **QUAN TRỌNG:** B-tree cho phép việc tìm kiếm nút lá diễn ra một cách
   nhanh chóng.

Thủ tục duyệt cây này là rất hiệu quả, đến mức nó được coi là sức mạnh
thứ nhất của chỉ mục. Nó có thể tìm ra bản ghi mong muốn gần như ngay
lập tức, ngay cả trên những cơ sở dữ liệu rất lớn. Điều này có được phần
lớn là nhờ cấu trúc cây là cân bằng, đảm bảo rằng số bước duyệt tới nút
lá là bằng nhau và tăng theo hàm log của kích thước dữ liệu. Tức là
chiều sâu của cây cân bằng tăng