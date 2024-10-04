# 1. Tổng Quan về MongoDB

MongoDB là một loại cơ sở dữ liệu **linh hoạt**, cho phép người dùng **lưu trữ dữ liệu** một cách **đơn giản** và **hiệu quả**. Thay vì sử dụng bảng (table) như trong các cơ sở dữ liệu truyền thống, MongoDB sử dụng các khái niệm như **Collection** và **Document**, để quản trị cơ sở dữ liệu **NoSQL**.

- **NoSQL** (*Not only SQL*): Được sử dụng thay thế cho cơ sở dữ liệu quan hệ truyền thống (Relational Database - RDB). NoSQL rất hữu ích khi làm việc với các tập dữ liệu lớn và phân tán.
- MongoDB là một công cụ quản lý thông tin theo hướng **document**, có khả năng lưu trữ và truy xuất dữ liệu hiệu quả.

Trong khi đó, **SQL** (*Structured Query Language*) là ngôn ngữ lập trình tiêu chuẩn dùng để quản lý các cơ sở dữ liệu **quan hệ**. Dữ liệu trong SQL được chuẩn hóa dưới dạng **schema** và **table**, với cấu trúc cố định.

## Ưu điểm của MongoDB

- **Tính linh hoạt**: MongoDB là hệ thống cơ sở dữ liệu **phi quan hệ**, cung cấp khả năng lưu trữ dữ liệu mà không cần tuân theo một mô hình quan hệ cụ thể.
- **Khả năng mở rộng**: Nhờ tính năng **sharding**, MongoDB cho phép phân chia dữ liệu thành nhiều phần và lưu trữ trên nhiều máy chủ.
- **Tốc độ truy xuất nhanh**: MongoDB có thể đáp ứng các yêu cầu truy vấn dữ liệu trong thời gian ngắn hơn so với các hệ thống cơ sở dữ liệu quan hệ truyền thống.
- **Tính khả dụng cao**: MongoDB hỗ trợ **sao lưu** và **phục hồi dữ liệu**, giúp bảo vệ dữ liệu khỏi rủi ro.
- **Dễ sử dụng**: MongoDB cung cấp các công cụ quản lý dữ liệu trực quan và dễ sử dụng, tối ưu hóa hiệu suất.
- **Dễ dàng tích hợp với Big Data**: MongoDB dễ dàng tích hợp với các hệ thống Big Data như **Hadoop**.

## Nhược điểm của MongoDB

- **Yêu cầu bộ nhớ cao**: Cần nhiều bộ nhớ để lưu trữ dữ liệu (*data storage*).
- **Giới hạn dung lượng tài liệu**: Không thể lưu trữ các tài liệu lớn hơn 16MB.
- **Hạn chế về Data Nesting**: BSON không cho phép nested data quá 100 cấp độ.

---

## a) Các Khái Niệm

### Khái Niệm Database
> **Database** là nơi lưu trữ các **Collection**. Mỗi cơ sở dữ liệu sẽ có một tập hợp các tệp riêng biệt trên máy chủ. Một máy chủ MongoDB có thể chứa nhiều cơ sở dữ liệu khác nhau.

### Khái Niệm Collection
> **Collection** là nhóm các **Document**. Tương tự như bảng (table) trong các hệ quản trị cơ sở dữ liệu khác. Một Collection thuộc về một cơ sở dữ liệu duy nhất và không có ràng buộc quan hệ như các hệ thống truyền thống, giúp việc truy xuất dữ liệu nhanh chóng. Trong một Collection, có thể lưu trữ nhiều loại dữ liệu khác nhau, không giống như các bảng cố định trong MySQL.

### Khái Niệm Document
> **Document** là đơn vị cơ bản trong MongoDB, có cấu trúc tương tự như **JSON**. Nó được tạo thành từ các cặp **key-value**. Document trong cùng một Collection không cần phải giống nhau về cấu trúc hay các trường dữ liệu. Điều này có nghĩa là mỗi Document có thể có các trường khác nhau và kiểu dữ liệu khác nhau.

<div align="center">

| &nbsp;&nbsp;&nbsp;&nbsp;RDBMS&nbsp;&nbsp;&nbsp;&nbsp; |&nbsp;&nbsp;&nbsp;&nbsp;MongoDB&nbsp;&nbsp;&nbsp;&nbsp;|
| --- | --- |
| Database | Database |
| Table | Collection |
| Row | Document |
| Column | Field |

<h4>Mối liên hệ giữa RDBMS và MongoDB</h4> 

</div>




