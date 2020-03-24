================================================
Tại sao sử dụng chỉ mục mà vẫn chậm? Phần 1
================================================

Mặc dù hiệu năng duyệt cây là rất cao, vẫn có những trường hợp mà quá trình tìm kiếm trên chỉ mục không nhanh như mong đợi. Sự mâu thuẫn này đã được nhắc đến với cái tên "sự thoái hóa chỉ mục" trong một khoảng thời gian dài. Lý do thực sự mà một vài câu truy vấn không có gì phức tạp lại có thể trở nên rất chậm ngay cả khi đã dùng chỉ mục được giải thích bằng những kiến thức mà ta đã học được trong chương này như sau.

Lý do đầu tiên khiến việc tìm kiếm chỉ mục trở nên chậm chạp là do trên danh sách nút lá có nhiều bản ghi có cùng giá trị khóa. Lấy ví dụ trong **Hình 1.3.** , có thể thấy có ít nhất hai bản ghi mang giá trị 57. Giả sử có rất nhiều bản ghi có giá trị khóa bằng giá trị khóa cần tìm kiếm, cơ sở dữ liệu cần phải duyệt tiếp theo chiều ngang trên danh sách nút lá để lấy về tất cả kết quả, bên cạnh việc duyệt cây theo chiều dọc. Quá trình duyệt ngang này có độ phức tạp thời gian lên đến ``O(n)`` , trong khi quá trình duyệt theo chiều dọc trên cây, như đã nói, chỉ có độ phức tạp là ``log(n)`` .

Yếu tố thứ hai khiến cho việc tìm kiếm trên chỉ mục trở nên chậm chạp là do quá trình truy cập bảng dữ liệu. Do dữ liệu trên bảng là không được sắp xếp, mỗi lần tìm kiếm thành công một bản ghi trên nút lá sẽ yêu cầu thêm một thao tác truy cập tới bảng dữ liệu, làm chậm tốc độ truy vấn dữ liệu.

Như vậy trong ba bước để truy vấn dữ liệu: (1) duyệt cây chỉ mục, (2) duyệt trên danh sách nút lá và (3) lấy dữ liệu từ bảng, chỉ có thao tác (1) là có giới hạn cận trên đối với số lượng khối dữ liệu phải truy cập trong khi thao tác (2) và (3) đôi khi đòi hỏi cần phải truy cập rất nhiều khối dữ liệu khác nhau, khiến cho thao tác tìm kiếm trở nên chậm chạp.

    **KHẢ NĂNG MỞ RỘNG CỦA HÀM LOGARIT**

    Trong toán học, logarit của một đối số với một cơ số cho trước là giá trị mà khi lấy cơ số nâng lên lũy thừa của giá trị đó ta thu được đối số ban đầu.

    Trong cây tìm kiếm, cơ số chính là số lượng bản ghi trong mỗi nút cành và số mũ là chiều sâu của cây. Ví dụ trong **Hình 1.2** . là một cây chứa tới 4 bản ghi mỗi node và chiều sâu cây bằng 3. Như vậy cây này có thể chứa tối đa lên tới 4 mũ 3 là 64 bản ghi tất cả. Nếu chiều sâu của cây tăng lên 1, số lượng bản ghi tối đa có thể chứa là 256. Với mỗi chiều nấc chiều sâu được thêm vào, số lượng bản ghi tăng lên gấp bốn. Do hàm logarit là nghịch đảo của hàm mũ, chiều sâu của cây tương ứng bằng logarit cơ số 4 của số lượng bản ghi.

    Sự tăng trưởng theo hàm log là rất rất chậm so với tốc độ tăng của đối số, điều này khiến cho cây trong ví dụ trên có thể chứa tới hàng triệu bản ghi với chiều sâu bằng 10. Chỉ mục trong thực tế còn hiệu quả hơn thế, khi mà số lượng bản ghi trong một khối (tương ứng với cơ số trong hàm log) lớn hơn nhiều so với con số 4 (thường là hàng trăm), khiến cây tìm kiếm trở nên rất nông (gần như không vượt quá 5 mức) và quá trình tìm kiếm diễn ra gần như tức thời.

Nguồn gốc của câu chuyện tưởng tượng về "chỉ mục chậm" xuất phát từ sự hiểu nhầm rằng quá trình truy xuất dữ liệu chỉ bao gồm thao tác duyệt cây, và cho rằng truy vấn chậm hẳn là do cây gặp vấn đề hoặc là không cân bằng. Thực tế, bạn có thể gửi yêu cầu cho hệ quản trị cơ sở dữ liệu để xem cách chúng thao tác đối với chỉ mục thế nào. Oracle đặc biệt quan tâm đến vấn đề này khi định nghĩa ra ba thao tác truy vấn trên chỉ mục:

``INDEX UNIQUE SCAN``: Chỉ thực hiện quá trình duyệt cây. Cơ sở dữ liệu Oracle thực hiện thao tác này chỉ khi một ràng buộc nào đó đảm bảo rằng thao tác tìm kiếm không trả về nhiều hơn một bản ghi

``INDEX RANGE SCAN``: Thực hiện thao tác duyệt cây và duyệt theo danh sách nút lá để tìm về tất cả các bản ghi khớp với truy vấn.

``TABLE ACCESS BY INDEX ROWID``: Thao tác này thực hiện lấy về tất cả các hàng từ bảng dữ liệu tương ứng với các bản ghi trả về bởi thao tác ``INDEX SCAN`` trước đó Điểm quan trọng là thủ tục ``INDEX RANGE SCAN`` có thể sẽ phải đọc một phần lớn của chỉ mục. Và vì mỗi hàng trong danh sách kết quả đòi hỏi sinh ra thêm một lần truy cập bản ghi, câu truy vấn có thể sẽ trở nên chậm chạp ngay cả khi sử dụng chỉ mục.
