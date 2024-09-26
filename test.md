
# TÌM HIỂU MONGODB

## Tổng quan MongoDB
MongoDB là một loại cơ sở dữ liệu linh hoạt cho phép người dùng lưu trữ dữ liệu theo cách đơn giản và hiệu quả. Thay vì sử dụng bảng như trong các cơ sở dữ liệu truyền thống, MongoDB sử dụng các khái niệm như **Collection** và **Document** để quản lý cơ sở dữ liệu NoSQL.

NoSQL (Not only SQL) được sử dụng thay thế cho cơ sở dữ liệu quan hệ (RDBMS). Cơ sở dữ liệu NoSQL rất hữu ích khi làm việc với các tập dữ liệu lớn và phân tán. MongoDB là một công cụ có thể quản lý dữ liệu theo hướng document và cho phép lưu trữ hoặc truy xuất thông tin.

**Khái niệm cơ bản:**

- **Database**: Nơi lưu trữ các Collection.
- **Collection**: Nhóm các Document, tương tự như bảng trong các hệ quản trị cơ sở dữ liệu khác.
- **Document**: Đơn vị cơ bản trong MongoDB, có cấu trúc giống JSON với các cặp key-value.

### So sánh giữa RDBMS và MongoDB

| RDBMS         | MongoDB    |
|---------------|------------|
| Database      | Database   |
| Table         | Collection |
| Row           | Document   |
| Column        | Field      |

## Lợi Thế Của MongoDB So Với RDBMS
- **Ít yêu cầu về cấu trúc**: MongoDB cho phép lưu trữ nhiều loại Document khác nhau trong cùng một Collection.
- **Cấu trúc rõ ràng**: Mỗi Document có cấu trúc dễ hiểu và không cần cấu trúc cố định như trong SQL.
- **Không cần Join phức tạp**: Không cần thực hiện các phép Join phức tạp như trong cơ sở dữ liệu quan hệ.
- **Khả năng truy vấn mạnh mẽ**: MongoDB hỗ trợ truy vấn linh hoạt với một ngôn ngữ tương tự SQL.
- **Dễ dàng mở rộng**: Cho phép mở rộng hệ thống một cách đơn giản.
- **Tối ưu hóa hiệu suất**: MongoDB sử dụng bộ nhớ trong để tăng tốc độ truy cập dữ liệu.

## Các thao tác cơ bản

```bash
# Tạo Database
use <tên_database>

# Xóa Database
db.dropDatabase()

# Tạo Collection
db.createCollection("<tên_collection>")

# Xóa Collection
db.<tên_collection>.drop()

# Chèn Document
db.<tên_collection>.insertOne(<document>)

# Truy vấn Document
db.<tên_collection>.find(<điều_kiện>)

# Cập nhật Document
db.<tên_collection>.updateOne(<điều_kiện>, <cập_nhật>)

# Xóa Document
db.<tên_collection>.deleteOne(<điều_kiện>)
```

## Mô Hình Replica Set

Replica Set là một nhóm các MongoDB server làm việc cùng nhau để đảm bảo tính sẵn có và độ tin cậy của dữ liệu. Nó cung cấp khả năng tự động sao chép và phục hồi dữ liệu.

### Cấu Trúc của Replica Set
- **Primary Node**: Node chính thực hiện các thao tác ghi dữ liệu.
- **Secondary Nodes**: Các node sao chép dữ liệu từ Primary.
- **Arbiter (nếu cần)**: Tham gia vào quá trình bầu chọn Primary mới khi cần.

### Quá Trình Hoạt Động
1. **Khởi tạo**: Một node được chỉ định làm Primary.
2. **Sao chép**: Secondary Nodes sao chép dữ liệu từ Primary.
3. **Bầu chọn**: Khi Primary gặp sự cố, các node sẽ bầu chọn Secondary để trở thành Primary mới.

