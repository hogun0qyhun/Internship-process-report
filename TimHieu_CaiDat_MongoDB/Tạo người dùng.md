### User có khả năng đọc viết
```Javascript
use myDatabase

db.createUser({
  user: "testUser",
  pwd: "password123",
  roles: [
    { role: "readWrite", db: "myDatabase" }
  ]
})

```
### Các quyền (roles) phổ biến trong MongoDB
*MongoDB có nhiều quyền khác nhau, tùy vào mức độ truy cập mà bạn muốn cấp cho người dùng:*

- `read`: Cho phép người dùng chỉ đọc dữ liệu trong một cơ sở dữ liệu cụ thể.
- `readWrite`: Cho phép người dùng đọc và ghi dữ liệu trong một cơ sở dữ liệu cụ thể.
- `dbAdmin`: Cho phép người dùng thực hiện các thao tác quản trị trên một cơ sở dữ liệu như tạo collection, index, v.v.
- `userAdmin`: Cho phép người dùng quản lý tài khoản người dùng trong cơ sở dữ liệu.
- `clusterAdmin`: Quyền quản lý cụm (cluster) trong MongoDB.
- `root`: Quyền truy cập và quản trị toàn bộ hệ thống (cơ sở dữ liệu admin).

###  Xác minh người dùng đã tạo

Sau khi tạo người dùng, kiểm tra xem người dùng đã được tạo thành công hay chưa bằng cách chạy lệnh:

```Javascript
db.getUsers()
```
### Cách tạo một role mới trong MongoDB

#### Tạo một role với quyền đọc và ghi trên một database
```Javascript
db.createRole({
  role: "readWriteRole",
  privileges: [
    {
      resource: { db: "mydb", collection: "" }, // Áp dụng cho tất cả các collection trong db 'mydb'
      actions: [ "find", "insert", "update", "remove" ] // Các quyền: tìm kiếm, thêm, cập nhật, xóa
    }
  ],
  roles: [] // Không thừa kế quyền từ role khác
})

```

####  Tạo một role với quyền quản trị

```Javascript
db.createRole({
  role: "adminRole",
  privileges: [
    {
      resource: { db: "admin", collection: "" }, // Áp dụng cho tất cả các collection trong db 'admin'
      actions: [ "createCollection", "dropCollection", "createIndex", "dropIndex" ] // Các quyền quản lý
    }
  ],
  roles: [] // Không thừa kế từ role nào khác
})

```

#### Tạo role thừa kế quyền từ role khác

```Javascript
db.createRole({
  role: "customAdminRole",
  privileges: [],
  roles: [
    { role: "readWrite", db: "mydb" },  // Thừa kế quyền readWrite từ mydb
    { role: "dbAdmin", db: "mydb" }     // Thừa kế quyền dbAdmin từ mydb
  ]
})

```
