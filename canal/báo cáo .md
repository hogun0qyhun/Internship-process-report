# Giới thiệu

## Mục đích:
Canal là một công cụ theo dõi thay đổi dữ liệu (CDC - Change Data Capture) trong MySQL. Nó giả lập giao thức binlog slave của MySQL để đọc binlog và truyền tải các thay đổi dữ liệu đến hệ thống khác. Canal được sử dụng để xây dựng hệ thống đồng bộ dữ liệu hoặc các pipeline xử lý dữ liệu real-time.
## Canal có các tính năng sau:

1. Hỗ trợ tất cả các nền tảng.
2. Hỗ trợ giám sát hệ thống chi tiết, được cung cấp bởi Prometheus .
3. Hỗ trợ phân tích cú pháp và đăng ký MySQL binlog bằng nhiều cách khác nhau, ví dụ như bằng GTID.
4. Hỗ trợ hiệu suất cao, đồng bộ dữ liệu theo thời gian thực.
5. Cả Canal Server và Canal Client đều hỗ trợ HA/Khả năng mở rộng, được cung cấp bởi Apache ZooKeeper
6. Hỗ trợ Docker. 

### Khái niệm:

- Canal là một open-source project do Alibaba phát triển, hỗ trợ đọc và phân tích binlog từ MySQL.
- Được thiết kế để theo dõi và xử lý thay đổi dữ liệu trong thời gian thực.

### Kiến trúc của Canal:

- Canal Server: Thành phần chính xử lý binlog từ MySQL.
- Canal Client: Thành phần nhận dữ liệu thay đổi từ Canal Server.
- Binlog Parser: Bộ phân tích dữ liệu từ binlog.

### Nguyên lý hoạt động:

- Canal kết nối với MySQL như một binlog slave.
- Binlog được đọc và phân tích thành các sự kiện (insert, update, delete).
- Dữ liệu đã phân tích được truyền đến các hệ thống tích hợp qua Canal Client.

___
# Cài đặt
- Đối với MySQL tự xây dựng, trước tiên cần kích hoạt chức năng ghi Binlog và định cấu hình định dạng binlog sang chế độ ROW.

``` bash  
[mysqld]
log-bin=mysql-bin # kích hoạt binlog
binlog-format=ROW # chọn chế độ ROW 
server_id=1 # Xác định cấu hình bản MySQL replaction，không trùng lặp slaveId của canal.
```
  > Lưu ý: Đối với Alibaba Cloud RDS cho MySQL, binlog được bật theo mặc định và tài khoản có quyền kết xuất binlog theo mặc định. Có thể bỏ qua bước này.
Sau khi khởi động lại cơ sở dữ liệu, chỉ cần kiểm tra xem my.cnfcấu hình có hiệu lực hay không:

- Tài khoản canal của MySQL được ủy quyền có quyền hoạt động như một Slave MySQL. Nếu đã có tài khoản, có thể ủy quyền cho nó.
``` sql
CREATE USER canal IDENTIFIED BY 'canal';  
GRANT SELECT, REPLICATION SLAVE, REPLICATION CLIENT ON *.* TO 'canal'@'%';
-- GRANT ALL PRIVILEGES ON *.* TO 'canal'@'%' ;
FLUSH PRIVILEGES;
```
- Tải xuống canal, truy cập trang `https://github.com/alibaba/canal/releases` , chọn gói cần tải xuống (lấy phiên bản 1.0.17 làm ví dụ)



















