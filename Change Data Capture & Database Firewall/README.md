### Change Data Capture (CDC)
> Change Data Capture là một kỹ thuật trong quản lý cơ sở dữ liệu dùng để theo dõi và ghi lại những thay đổi dữ liệu xảy ra trong cơ sở dữ liệu. Nó cho phép xác định các bản ghi đã được thêm, sửa đổi hoặc xóa mà không cần phải quét toàn bộ dữ liệu. Điều này giúp giảm thiểu thời gian xử lý và cải thiện hiệu suất khi làm việc với lượng dữ liệu lớn.

- CDC thường được sử dụng trong các trường hợp như:

  - Tích hợp dữ liệu giữa các hệ thống khác nhau (ví dụ: từ cơ sở dữ liệu giao dịch đến kho dữ liệu).
  - Sao chép dữ liệu thời gian thực từ nguồn sang hệ thống khác để đồng bộ hóa dữ liệu.
  - Phân tích dữ liệu thời gian thực hoặc gần thời gian thực mà không làm ảnh hưởng đến hệ thống nguồn.
- Thành phần chính của CDC
  - Nguồn thay đổi dữ liệu: Cơ sở dữ liệu nơi các hoạt động như INSERT, UPDATE, DELETE diễn ra.
  - Cơ chế phát hiện thay đổi: Ghi lại hoặc phát hiện các thay đổi.
Ví dụ: Log-based CDC (sử dụng log bin của MySQL hoặc redo log của Oracle).
Trigger-based CDC (sử dụng trigger trong cơ sở dữ liệu để ghi lại thay đổi).

  - Mục tiêu lưu trữ: Nơi dữ liệu thay đổi được chuyển tiếp, như hệ thống phân tích hoặc kho dữ liệu.
  - Công cụ CDC: Một số công cụ như Debezium, Canal, hoặc các giải pháp tích hợp sẵn trong các hệ quản trị cơ sở dữ liệu như SQL Server.

#### Lợi ích của Change Data Capture
- Giảm tải cho hệ thống: Không cần phải thực hiện các truy vấn quét toàn bộ dữ liệu.
- Cải thiện hiệu suất: Đồng bộ hóa dữ liệu gần thời gian thực mà không làm gián đoạn hệ thống nguồn.
- Tối ưu hóa việc phân tích dữ liệu: Cập nhật dữ liệu mới nhất cho hệ thống phân tích mà không bị trễ.

### Databse Firewall
> Database firewall một giải pháp bảo mật quan trọng để kiểm soát và giám sát các truy cập vào cơ sở dữ liệu, giúp ngăn chặn các hành vi xâm nhập trái phép và bảo vệ dữ liệu nhạy cảm. Nó thường hoạt động bằng cách phân tích các truy vấn SQL và áp dụng các chính sách bảo mật đã định nghĩa trước.
 - Chức năng chính:
    - Kiểm tra và lọc các truy vấn SQL trước khi chúng được thực hiện trong cơ sở dữ liệu.
    - Phát hiện các mẫu truy vấn đáng ngờ hoặc không hợp lệ.
    - Ghi lại hoạt động để kiểm tra và phân tích sau này.
 - Phương pháp lọc:
    - Dựa trên whitelist: Chỉ cho phép các truy vấn đã biết và được xác định là an toàn.
    - Dựa trên blacklist: Chặn các truy vấn có mẫu hoặc từ khóa đáng ngờ (như DROP, DELETE không có điều kiện).

####  So sánh giữa Database Firewall và CDC (Change Data Capture)
|Tính năng|Database Firewall|CDC(Canal, Debezium, ..)|
|---|---|---|
|Mục đích chính|Bảo mật và kiểm soát truy cập|Theo dõi và ghi lại thay đổi dữ liệu|
|Tác động đến truy vấn|Chặn hoặc cho phép truy vấn dựa trên chính sách|Ghi nhận thay đổi sau khi truy vấn được thực thi|
|Ứng dụng chính|Ngăn chặn SQL Injection, bảo mật truy cập cơ sở dữ liệu|Đồng bộ dữ liệu giữa các hệ thống hoặc phát sự kiện
|Khả năng triển khai|Phải được cấu hình giữa ứng dụng và cơ sở dữ liệu|Đọc từ binlog hoặc transaction log của cơ sở dữ liệu








