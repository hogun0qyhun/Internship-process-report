
# Tìm hiểu MongoDB

## 1. Tổng quan MongoDB
MongoDB là một loại cơ sở dữ liệu linh hoạt, cho phép người dùng lưu trữ dữ liệu theo cách đơn giản và hiệu quả. Thay vì sử dụng bảng như trong các cơ sở dữ liệu truyền thống, MongoDB sử dụng các khái niệm như Collection và Document, dùng để quản trị cơ sở dữ liệu NoSQL.
NoSQL (Not only SQL) được sử dụng thay thế cho cơ sở dữ liệu quan hệ (Relational Database – RDB) truyền thống. Cơ sở dữ liệu NoSQL khá hữu ích trong khi làm việc với các tập dữ liệu phân tán lớn. MongoDB là một công cụ có thể quản lý thông tin hướng document cũng như lưu trữ hoặc truy xuất thông tin.
Trong khi đó, ngôn ngữ truy vấn có cấu trúc (SQL) là ngôn ngữ lập trình được tiêu chuẩn hóa, dùng để quản lý cơ sở dữ liệu quan hệ. Dữ liệu được chuẩn hóa SQL dưới dạng schema và table và mọi table đều có cấu trúc cố định.

### a) Các khái niệm

#### Khái niệm Database
Database là nơi lưu trữ các Collection. Mỗi cơ sở dữ liệu sẽ có một tập hợp các tệp riêng biệt trên máy chủ. Một máy chủ MongoDB có thể chứa nhiều cơ sở dữ liệu khác nhau.

#### Khái niệm Collection
Collection là nhóm các Document. Tương tự như một bảng trong các hệ quản trị cơ sở dữ liệu khác. Một Collection thuộc về một cơ sở dữ liệu duy nhất và không có ràng buộc quan hệ như các hệ thống truyền thống, giúp việc truy xuất dữ liệu diễn ra nhanh chóng. Điều đặc biệt là trong một Collection, có thể lưu trữ nhiều loại dữ liệu khác nhau, không giống như các bảng cố định trong MySQL. Các Document trong một Collection có thể có các trường khác nhau, nhưng thường có điểm chung hoặc liên quan đến nhau.

#### Khái niệm Document
Document là đơn vị cơ bản trong MongoDB, có cấu trúc tương tự như JSON. Nó được tạo thành từ các cặp key-value. Document trong cùng một Collection không cần phải giống nhau về cấu trúc hay các trường dữ liệu. Điều này có nghĩa là mỗi Document có thể có các trường khác nhau và kiểu dữ liệu khác nhau.
