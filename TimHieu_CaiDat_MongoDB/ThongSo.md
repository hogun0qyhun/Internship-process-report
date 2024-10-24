### Các thông số trong MongoDB

- *`Insert`*: Số lượng thao tác insert (chèn dữ liệu) trên giây. Đây là số liệu quan trọng để đánh giá tần suất ghi dữ liệu mới vào MongoDB. Giá trị cao có thể biểu thị nhiều ghi dữ liệu đang diễn ra.

- *`Query`*: Số lượng thao tác query (truy vấn) trên giây. Số lượng truy vấn này giúp đánh giá mức độ tải của hệ thống từ các ứng dụng hoặc người dùng đang tìm kiếm dữ liệu.

- *`Update`*: Số lượng thao tác update (cập nhật) trên giây. Nếu số liệu này cao, hệ thống có thể đang xử lý nhiều cập nhật thông tin, đặc biệt khi có các thay đổi dữ liệu lớn.

- *`Delete`*: Số lượng thao tác delete (xóa) trên giây. Đây là chỉ số về số lượng tài liệu bị xóa trong MongoDB. Thao tác này quan trọng để theo dõi nếu bạn có các quy trình làm sạch dữ liệu hoặc thao tác xóa thường xuyên.

- *`Getmore`*: Số lượng thao tác getmore trên giây. Khi một truy vấn trả về nhiều kết quả hơn giới hạn kích thước batch, MongoDB sẽ thực hiện các thao tác getmore để trả về phần còn lại của dữ liệu.

- *`Command`*: Số lượng lệnh MongoDB khác được thực thi trên giây. Bao gồm các thao tác như xác thực, đếm, tìm kiếm chỉ số, và nhiều thao tác quản lý khác. Đây là chỉ số cho thấy mức độ tương tác tổng thể với MongoDB.

- *`Dirty`*: Tỷ lệ phần trăm bộ nhớ dirty, là các trang trong bộ nhớ RAM đã bị thay đổi nhưng chưa được ghi xuống ổ đĩa. Nếu giá trị này cao, điều đó có thể biểu thị rằng hệ thống chưa hoàn tất ghi dữ liệu từ bộ nhớ tạm xuống ổ đĩa.

- *`Used`*: Tỷ lệ phần trăm của bộ nhớ cache đang được sử dụng. Giá trị cao ở đây có thể chỉ ra rằng hệ thống đang tận dụng bộ nhớ RAM tối đa cho dữ liệu của MongoDB.

- *`Flushes`*: Số lần flush trên giây, đây là quá trình ghi dữ liệu từ bộ nhớ xuống ổ đĩa. Nếu giá trị này lớn, điều đó có nghĩa là hệ thống đang ghi dữ liệu xuống đĩa thường xuyên.

- *`Vsize`* (Virtual Memory Size): Dung lượng bộ nhớ ảo mà MongoDB đang sử dụng. Số này bao gồm cả bộ nhớ vật lý và bộ nhớ swap. Giá trị quá lớn có thể biểu thị rằng MongoDB đang sử dụng nhiều bộ nhớ hơn dự kiến.

- *`Res` (Resident Memory Size)*: Dung lượng bộ nhớ vật lý mà MongoDB đang sử dụng. Nếu giá trị này cao, có nghĩa là hệ thống MongoDB của bạn đang chiếm dụng nhiều RAM.

- *`QRW` (Queue Reads/Writes)*: Số lượng thao tác đọc (Read) và ghi (Write) đang chờ xử lý trong hàng đợi. Nếu giá trị này lớn, có thể có sự nghẽn cổ chai trong việc xử lý đọc/ghi của MongoDB.

- *`ARW` (Active Reads/Writes)*: Số lượng thao tác đọc (Read) và ghi (Write) đang hoạt động. Điều này phản ánh khối lượng công việc hiện tại của hệ thống.

- *`Net_in` (Network Input)*: Lượng dữ liệu nhận được qua mạng, tính bằng bytes. Chỉ số này phản ánh lưu lượng dữ liệu đến MongoDB, biểu thị số lượng truy vấn hoặc thao tác ghi dữ liệu từ các ứng dụng.

- *`Net_out` (Network Output)*: Lượng dữ liệu được gửi ra ngoài qua mạng, tính bằng bytes. Điều này cho thấy lưu lượng dữ liệu mà MongoDB gửi trả lại các truy vấn hoặc kết quả ra ngoài.

- *`Conn` (Connections)*: Số lượng kết nối hiện tại đến MongoDB. Giá trị cao có thể chỉ ra rằng có nhiều kết nối từ các ứng dụng hoặc người dùng đang sử dụng hệ thống.

- *`Set`*: Tên của replica set mà MongoDB đang thuộc về. Thông số này giúp nhận diện replica set hiện tại nếu bạn đang chạy trong môi trường replica.

- *`Repl`* (Role in Replica Set): Vai trò của node MongoDB trong replica set (ví dụ: primary, secondary, hoặc arbiter). Điều này quan trọng trong việc giám sát phân phối nhiệm vụ trong replica set.

- *`Time`*: Thời gian hiện tại khi thống kê được lấy. Điều này giúp theo dõi thời gian và kiểm tra các chỉ số theo thời gian thực.
