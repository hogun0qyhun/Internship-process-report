# SAN

## 1. Tổng quan

- SAN (Storage area network) là kỹ thuật lưu trữ dữ liệu dạng block-level thông qua kết nối mạng.
- SAN được sử dụng để tăng khả năng lưu trữ của thiết bị, như disk arrays, tape libraries, etc.
- SAN thường có mạng lưới lưu trữ riêng mà không thông qua mạng LAN với các thiết bị khác.
- SAN sử dụng các giao thức sau: Fibre Channel, iSCSI, ATA over Ethernet (AoE) and HyperSCSI.

### 1.1. Khái niệm 

- SAN (Storage Area Network) là một mạng quang riêng tốc độ cao dùng cho việc truyền dữ liệu giữa các máy chủ tham gia vào hệ thống lưu trữ cũng như giữa các thiết bị lưu trữ với nhau. SAN cho phép thực hiện quản lý tập trung và cung cấp khả năng chia sẻ dữ liệu và tài nguyên lưu trữ. Hầu hết mạng SAN hiện nay dựa trên công nghệ kênh cáp quang, cung cấp cho người sử dụng khả năng mở rộng, hiệu năng và tính sẵn sàng cao.

### 1.2. Đặc điểm

- Hệ thống SAN được chia làm hai mức: mức vật lý và logic
   - Mức vật lý: mô tả sự liên kết các thành phần của mạng tạo ra một hệ thống lưu trữ đồng nhất và có thể sử dụng đồng thời cho nhiều ứng dụng và người dùng.
   - Mức logic: bao gồm các ứng dụng, các công cụ quản lý và dịch vụ được xây dựng trên nền tảng của các thiết bị lớp vật lý, cung cấp khả năng quản lý hệ thống SAN.

<img src="Picture/san11111.webp" />

- Ưu điểm của hệ thống SAN

   - Có khả năng sao lưu dữ liệu với dung lượng lớn và thường xuyên mà không làm ảnh hưởng đến lưu lượng thông tin trên mạng.
   - SAN đặc biệt thích hợp với các ứng dụng cần tốc độ và độ trễ nhỏ.
   - Dữ liệu luôn ở mức độ sẵn sàng cao.
   - Dữ liệu được lưu trữ thống nhất, tập trung và có khả năng quản lý cao. Có khả năng khôi phục dữ liệu nếu có xảy ra sự cố.
   - Hỗ trợ nhiều giao thức, chuẩn lưu trữ khác nhau như: iSCSI, FCIP, DWDM...

- Có khả năng mở rộng tốt trên cả phương diện số lượng thiết bị, dung lượng hệ thống cũng như khoảng cách vật lý.

## 2. Công nghệ kết nối iSCSIs

- iSCSI hay Internet Small Computer Systems Interface là một giao thức lớp vận chuyển hoạt động dựa trên TCP/IP, cho phép các thiết bị lưu trữ truyền dữ liệu qua mạng IP thay vì sử dụng đường truyền cáp quang như Fibre Channel.

- Các thành phần chính của iSCSI bao gồm:

   - iSCSI initiator: Là phần mềm hoặc card mạng được cài đặt trên máy chủ, cho phép máy chủ truy cập các thiết bị lưu trữ iSCSI.
   - iSCSI target: Là thiết bị lưu trữ hỗ trợ giao thức iSCSI, được kết nối với mạng LAN thông qua adapter đặc biệt.

Trong mạng SAN sử dụng iSCSI, các máy chủ được kết nối với iSCSI target thông qua mạng LAN. Các iSCSI initiator trên máy chủ sử dụng giao thức TCP/IP để truyền dữ liệu đến iSCSI target. Các iSCSI target cũng được kết nối với mạng LAN thông qua adapter chuyên dụng và sử dụng giao thức iSCSI để truyền dữ liệu đến các máy chủ.

<img src="Picture/san2.jpg" />

### 2.1. iCSI protocol

- iSCSI (Internet Small Computer Systems Interface) là là giao thức Internet thuộc tầng application để lưu trữ thông qua Internet. Nó cung cấp truy cập block-level để truy cập thiết bị lưu trữ.
- iSCSI được sử dụng để truyền data qua LAN, WAN hoặc Internet.
- Một số khái niệm:
   - Initiator: là 1 iSCSI client.
   - Target: là nơi chứa thiết bị lưu trữ data gọi – iSCSI server.
- iSCSI sử dụng giao thức TCP (tiêu biểu là port 860 và 3260)

## 3. LAB iSCSI protocol

### 3.1. Tổng quan

Mô hình:

<img src="Picture/SAN.png" />

Trong đó:

   - iSCSI Initiator: là Client sử dụng giao thức iSCSI để giao tiếp và sử dụng thiết bị lưu trữ bên Target.
   - iSCSI Target: là Server chứa thiết bị lưu trữ và được truy cập bởi client.
   - 2 máy đều cài hệ điều hành Ubuntu Server 16.04

Cài đặt:

   - iSCSI Initiator : Dùng gói “open-iscsi”.
       - File cấu hình trong thư mục /etc/iscsi gồm:
           - iscsid.conf: File cấu hình cho iscsi daemon. Nó đọc để startup.
           - initiatorname.iscsi: Tên của initator, daemon reds at the startup.
           - nodes directory: Thư mục chứa nodes và targets của họ.
           - send_targets directory: Thư mục chứa các targets được discovered.

open-iscsi được điều khiển bởi câu lệnh iscsiadm. Câu lệnh có thể discover các target, login/logout các target, display thông tin các session.

   - iSCSI target: Dùng gói “iscsitarget”
       - File cấu hình bao gồm:
           - Các file trong thư mục /etc/iet
           - File /etc/default/iscsitarget
