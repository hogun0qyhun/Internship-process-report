## Canal Admin
Canal admin được thiết kế để cung cấp khả năng quản lý cấu hình tổng thể, vận hành và bảo trì nút cũng như các chức năng định hướng vận hành khác cho kênh. Nó cũng cung cấp giao diện vận hành WebUI tương đối thân thiện để tạo điều kiện cho nhiều người dùng hoạt động nhanh chóng và an toàn hơn.
### Tải xuống Canal-admin, truy cập trang phát hành, chọn gói cần tải xuống, lấy phiên bản 1.1.4 làm ví dụ
```bash
wget https://github.com/alibaba/canal/releases/download/canal-1.1.4/canal.admin-1.1.4.tar.gz
```
- Sửa đổi cấu hình
``` bash
vi conf/application.yml
```
```bash
server:
  port: 8089
spring:
  jackson:
    date-format: yyyy-MM-dd HH:mm:ss
    time-zone: GMT+8

spring.datasource:
  address: 127.0.0.1:3306
  database: canal_manager
  username: canal
  password: canal
  driver-class-name: com.mysql.jdbc.Driver
  url: jdbc:mysql://${spring.datasource.address}/${spring.datasource.database}?useUnicode=true&characterEncoding=UTF-8&useSSL=false
  hikari:
    maximum-pool-size: 30
    minimum-idle: 1

canal:
  adminUser: admin
  adminPasswd: admin
```
- Khởi tạo dữ liệu: đăng nhập vào Mysql và khởi tạo dữ liệu theo tập lệnh SQL trong `canal_manager.sql`
- Khởi động và xem nhật kí Admin
  ```bash
    vi logs/admin.log
    2019-08-31 15:43:38.162 [main] INFO  o.s.boot.web.embedded.tomcat.TomcatWebServer - Tomcat initialized with port(s): 8089 (http)
    2019-08-31 15:43:38.180 [main] INFO  org.apache.coyote.http11.Http11NioProtocol - Initializing ProtocolHandler ["http-nio-8089"]
    2019-08-31 15:43:38.191 [main] INFO  org.apache.catalina.core.StandardService - Starting service [Tomcat]
    2019-08-31 15:43:38.194 [main] INFO  org.apache.catalina.core.StandardEngine - Starting Servlet Engine: Apache Tomcat/8.5.29
    ....
    2019-08-31 15:43:39.789 [main] INFO  o.s.w.s.m.m.annotation.ExceptionHandlerExceptionResolver - Detected @ExceptionHandler methods in customExceptionHandler
    2019-08-31 15:43:39.825 [main] INFO  o.s.b.a.web.servlet.WelcomePageHandlerMapping - Adding welcome page: class path resource [public/index.html]
  ```
  - Khởi động thành công, truy cập thông qua http://127.0.0.1:8089/. Mật khẩu mặc định là: admin/123456.
 
### Giao diện quản lý
 <div align="center">
  <img src="picture/Picture1.png" width="490" height="250" />
 <h4>Mô hình Mongo lưu trữ dữ liệu</h4>
 </div>
