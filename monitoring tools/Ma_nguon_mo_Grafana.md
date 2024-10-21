# Grafana

<div align="center">
  <img src="https://github.com/hogun0qyhun/Internship-process-report/blob/main/monitoring%20tools/Zabbix%20tool/picture/grafana-demo-dashboard%20.png" />
</div>
Grafana là một công cụ giám sát và trực quan hóa dữ liệu mã nguồn mở, được thiết kế để cung cấp cái nhìn sâu sắc về dữ liệu hệ thống, ứng dụng, và các cơ sở dữ liệu khác nhau thông qua các bảng điều khiển (dashboards) đẹp mắt và dễ sử dụng. Nó chủ yếu được sử dụng để giám sát hiệu suất hệ thống, cơ sở dữ liệu, ứng dụng và hạ tầng IT, nhưng có thể tùy chỉnh để phục vụ nhiều mục đích khác nhau.




___
### Tổng quan về các tính năng chính của Grafana

1.  Dashboards tương tác:

>Grafana cho phép người dùng tạo các bảng điều khiển tùy chỉnh với nhiều loại biểu đồ khác nhau như biểu đồ đường, biểu đồ cột, biểu đồ hình tròn, gauges (đồng hồ), heatmaps (biểu đồ nhiệt), và nhiều loại biểu đồ khác. Người dùng có thể chọn cách trình bày dữ liệu phù hợp với yêu cầu.

2.  Hỗ trợ nhiều nguồn dữ liệu:

>Một trong những điểm mạnh của Grafana là khả năng kết nối với nhiều nguồn dữ liệu khác nhau, bao gồm Prometheus, InfluxDB, Graphite, Elasticsearch, MySQL, PostgreSQL, MongoDB, và nhiều dịch vụ khác. Nó có thể hiển thị dữ liệu từ nhiều nguồn cùng một lúc trên một dashboard duy nhất.

3.  Cảnh báo (Alerting):
  
>Grafana có hệ thống cảnh báo mạnh mẽ, cho phép người dùng thiết lập các cảnh báo dựa trên ngưỡng giá trị của dữ liệu. Khi một giá trị nào đó vượt qua ngưỡng, Grafana có thể gửi thông báo qua email, Slack, hoặc các công cụ thông báo khác để người quản trị biết và can thiệp kịp thời.

4.  Plugin và khả năng mở rộng:
  
>Grafana có hệ sinh thái plugin phong phú, hỗ trợ các nguồn dữ liệu, bảng điều khiển, và các tính năng bổ sung như giám sát thời gian thực, tích hợp với các hệ thống khác, và cung cấp nhiều lựa chọn trực quan hóa hơn.

5.  Khả năng chia sẻ:

>Grafana cho phép chia sẻ dashboards một cách dễ dàng. Người dùng có thể tạo và xuất dashboard dưới dạng link, hoặc nhúng vào các trang web khác. Điều này rất hữu ích khi bạn muốn chia sẻ báo cáo giám sát với các bên liên quan mà không cần cấp quyền truy cập trực tiếp vào hệ thống.

6.  Quản lý người dùng và quyền hạn:

>Grafana hỗ trợ quản lý người dùng và cấp quyền khác nhau cho các thành viên trong đội. Các vai trò có thể được chỉ định cho người dùng để quản lý quyền truy cập vào các dashboards hoặc cấu hình hệ thống.

7.  Cộng đồng và mã nguồn mở:

>Grafana là một dự án mã nguồn mở và có một cộng đồng lớn đóng góp các tính năng mới, báo lỗi, và phát triển các plugin hữu ích. Điều này giúp Grafana liên tục phát triển và cập nhật.
___
### Các ứng dụng và lĩnh vực sử dụng
  
- *Giám sát hệ thống và hạ tầng*

Grafana thường được sử dụng cùng với Prometheus để giám sát hiệu suất của hệ thống máy chủ, bộ nhớ, CPU, băng thông mạng, và các thông số khác của hệ thống IT.

- *Giám sát cơ sở dữ liệu*

Grafana sử dụng để theo dõi hiệu suất của các cơ sở dữ liệu như MySQL, PostgreSQL, MongoDB, và nhiều cơ sở dữ liệu khác, để đảm bảo rằng chúng hoạt động hiệu quả.

- *Giám sát ứng dụng*

Khi được tích hợp với các công cụ giám sát ứng dụng như Prometheus hoặc Zabbix, Grafana giúp giám sát hiệu suất và độ tin cậy của ứng dụng.

- *Theo dõi IoT*

Với khả năng hỗ trợ nhiều nguồn dữ liệu, Grafana còn được sử dụng để theo dõi dữ liệu từ các thiết bị IoT, mạng cảm biến, và hệ thống tự động hóa.
___

#### Ưu điểm của Grafana

 - Dễ dàng sử dụng và tùy chỉnh: Giao diện trực quan và dễ sử dụng, giúp người dùng dễ dàng tạo và quản lý các dashboards.
  
 - Tích hợp đa dạng nguồn dữ liệu: Hỗ trợ nhiều loại cơ sở dữ liệu và dịch vụ khác nhau, tạo sự linh hoạt trong việc thu thập và phân tích dữ liệu.
  
 - Cộng đồng lớn: Là một dự án mã nguồn mở với sự hỗ trợ của cộng đồng, Grafana liên tục phát triển và có nhiều tài liệu hỗ trợ.

#### Hạn chế

 - Yêu cầu cấu hình: Việc cấu hình Grafana với các nguồn dữ liệu khác nhau có thể phức tạp đối với người mới bắt đầu.
  
 - Giới hạn trong việc phân tích dữ liệu phức tạp: Mặc dù Grafana rất mạnh trong việc trực quan hóa, nó có thể không mạnh bằng các công cụ BI (Business Intelligence) chuyên dụng trong việc phân tích dữ liệu phức tạp.


*Grafana là một công cụ quan trọng giúp tối ưu hóa việc giám sát và trực quan hóa dữ liệu, được sử dụng rộng rãi trong nhiều lĩnh vực như DevOps, quản trị hệ thống, và giám sát ứng dụng.*

___

### Cài đặt Grafana

- Bước 1 – Vô hiệu hóa SELinux

``` bash
setenforce 0
```

- Bước 2 – cài đặt Grafana bằng YUM Repository

Sử dụng Yum để cài đặt các gói ứng dụng `vi /etc/yum.repos.d/grafana.repo`.

``` bash
[grafana]
name=grafana
baseurl=https://packages.grafana.com/oss/rpm
repo_gpgcheck=1
enabled=1
gpgcheck=1
gpgkey=https://packages.grafana.com/gpg.key
sslverify=1
sslcacert=/etc/pki/tls/certs/ca-bundle.crt
```
- Bước 3 – Install Grafana
```bash
sudo yum install grafana 
```

- Bước 4 – Install additional font packages - Cài đặt bổ sung cần thiết
Bước 5 – Enable Grafana Service
```bash
yum install fontconfig
yum install freetype*
yum install urw-fonts
```

- Bước 5 – Khởi động Grafana Service

<div align="center">
  <img src="https://github.com/hogun0qyhun/Internship-process-report/blob/main/monitoring%20tools/Zabbix%20tool/picture/Screenshot%202024-10-21%20111902.png" />
</div>




