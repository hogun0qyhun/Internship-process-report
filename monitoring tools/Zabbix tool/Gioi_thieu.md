# Zabbix
___
## 1. Tổng quan về Zabbix

  Zabbix là một công cụ mã nguồn mở nổi tiếng giải quyết cho ta các vấn đề về giám sát – là phần mềm sử dụng các tham số của một mạng, tình trạng và tính toàn vẹn của Server cũng như các thiết bị mạng.

  Zabbix sử dụng một cơ chế thống báo linh hoạt và khả năng tuỳ biến cao cho phép người dùng cấu hình email hoặc sms để cảnh báo dựa trên sự kiện được ta thiết lập sẵn. Ngoài ra Zabbix cung cấp báo cáo và dữ liệu chính xác dựa trên cơ sở dữ liệu. Điều này khiến cho Zabbix trở nên lý tưởng hơn, thích hợp phục vụ cho hệ thống mạng tầm trung và lớn của các doanh nghiệp hiện tại với mức chi phí đầu tư vừa phải.

  Zabbix được sáng lập bởi Alexei Vladishev và hiện tại được phát triển cũng như hỗ trợ bởi tổ chức Zabbix SIA. Zabbix được viết và phát hành dưới bản quyền General Public License GPL phiên bản 2

  Tất cả báo cáo, thống kê cũng như cấu hình thông số của Zabbix có thể dễ dàng truy cập qua giao diện web tinh tế đẹp mắt. Giúp chúng ta theo dõi được tình trạng hệ thống thiết bị server, dịch vụ,..
<div align="center">
  <img src="https://github.com/hogun0qyhun/Internship-process-report/blob/main/monitoring%20tools/Zabbix%20tool/picture/zabbix-monitor-edit-1.png" />
</div>

___

 ## 2. Ưu và nhược điểm của Zabbix
### 2.1 Ưu điểm

- Giám sát, tự động tìm phát hiện server và hệ thống mạng.
- Hỗ trợ server cài đặt trên dòng hệ điều hành Unix/Linux.
- Hỗ trợ máy trạm client nhiều hệ điều hành.
- Giao diện web cực kì tinh tế và đẹp mắt.
- Thông báo sự cố qua email, OTP App và SMS.
- Mã nguồn mở, chi phí đầu tư thấp.
- Biểu đổ theo dõi và báo cáo qua giao diện.
- Kiểm soát theo dõi việc đăng nhập.
- Linh hoạt trong phân quyền người dùng.
- Nhiều Plugin hỗ trợ.

<div align="center">
  <img src="https://raw.githubusercontent.com/hogun0qyhun/Internship-process-report/main/monitoring%20tools/Zabbix%20tool/picture/image.png" />
</div>


### 2.2 Nhược điểm.

- Không có giao diện web mobile hỗ trợ.
- Không phù hợp với hệ thống mạng lớn, nhiều thiết bị client cần giám sát. Lúc này phát sinh vấn đề hiệu suất về PHP và Database, v..v..
- Thiết kế template/alerting rule đôi khi khá phức tạp.

___

## 3. Các thành phần của hệ thống giám sát dịch vụ Zabbix.

- **Zabbix Server**: là ứng dụng chương trình dịch vụ chính của dịch vụ Zabbix. Zabbix Server sẽ chịu trách nhiệm cho các hoạt động kiểm tra dịch vụ mạng từ xa, thu thập thông tin, lưu trữ, hiển thị, cảnh báo,… từ đó các quản trị viên có thể thao tác giám sát hệ thống tốt nhất.

- **Zabbix Proxy**: là một máy chủ được dùng cho việc quản lý nhiều nhánh hệ thống ở xa, hoặc ở các lớp mạng khác nhau. Từ Zabbix Proxy sẽ thu thập các thông tin thiết bị mạng rồi chuyển tiếp về cho máy chủ dịch vụ chính Zabbix Server.

- **Zabbix Agent**: để giám sát chủ động các thiết bị cục bộ và các ứng dụng (ổ cứng, bộ nhớ, …) trên hệ thống mạng. Zabbix Agent sẽ được cài lên trên Server và từ đó Agent sẽ thu thập thông tin hoạt động từ Server mà nó đang chạy và báo cáo dữ liệu này đến Zabbix Server để xử lý.

- **Giao diện web**: cung cấp giao diện web trên nền tảng mã nguồn PHP cùng phong cách metro tinh tế. Hiện tại có thể xem Zabbix là một trong những ứng dụng có giao diện đẹp nhất, thiết kế vị trí tính năng bắt mắt và hợp lý.

<div align="center">
  <img src="https://github.com/hogun0qyhun/Internship-process-report/blob/main/monitoring%20tools/Zabbix%20tool/picture/zabbix1.png" />
</div>

___

## 4. Cài đặt

1. Cài đặt Zabbix repository

```bash
wget https://repo.zabbix.com/zabbix/7.0/ubuntu/pool/main/z/zabbix-release/zabbix-release_latest+22.04_all.deb
dpkg -i zabbix-release_latest+22.04_all.deb
apt update
```

2. Cài đặt Zabbix server, frontend, agent
```bash 
apt install zabbix-server-mysql zabbix-frontend-php zabbix-apache-conf zabbix-sql-scripts zabbix-agent
```

3. Install MariaDB
```bash

sudo apt -y install mariadb-server mariadb-client
```

4. Cấu hình MariaDB
```bash
mysql_secure_installation
```

5. Khởi động lại dịch vụ MariaDB
```bash
systemctl status mariadb
systemctl enable mariadb
systemctl restart mariadb
```
6. Tạo initial database
```bash
# mysql -uroot -p
(Password tạo ở bước 4)
mysql> create database zabbix character set utf8mb4 collate utf8mb4_bin;
create user zabbix@localhost identified by '12345678a';
grant all privileges on zabbix.* to zabbix@localhost;
set global log_bin_trust_function_creators = 1;
mysql> quit;

zcat /usr/share/zabbix-sql-scripts/mysql/server.sql.gz | mysql --default-character-set=utf8mb4 -uzabbix -p zabbix

# mysql -uroot -p
password
mysql> set global log_bin_trust_function_creators = 0;
mysql> quit;
```
7. Sửa file conf
```bash

vim /etc/zabbix/zabbix_server.conf
Tìm dòng DBPassword thay bằng password ở bước 4: DBPassword=password
```
8.  Start Zabbix server và agent processes
```bash

systemctl restart zabbix-server zabbix-agent apache2
systemctl enable zabbix-server zabbix-agent apache2
```
___

## 5. Cấu hình giao diện người dùng web

Mở giao diện người dùng Zabbix mới cài đặt bằng URL: http://server_ip_or_dns_name/zabbix


























 
