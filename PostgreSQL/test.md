## Cài đặt MongoDB và Replica Set
### a) Môi trường cài đặt 
- **Hệ điều hành**: Oracle Linux 7.9
 <div align="center">
  <img src="https://github.com/hogun0qyhun/Internship-process-report/blob/main/TimHieu_CaiDat_MongoDB/picture/Picture4.png" width="350" height="250" />
 </div>

- **Công cụ**: VMware Workstation Pro, MobaXterm
<div align="center">
  <table>
    <tr>
      <td>
        <img src="https://github.com/hogun0qyhun/Internship-process-report/blob/main/TimHieu_CaiDat_MongoDB/picture/Picture5.png" alt="Picture 5" style="width: 300px;"/>
      </td>
      <td>
        <img src="https://github.com/hogun0qyhun/Internship-process-report/blob/main/TimHieu_CaiDat_MongoDB/picture/Picture6.png" alt="Picture 6" style="width: 300px;"/>
      </td>
    </tr>
  </table>
</div>

- **Phiên bản PostgreSQL**: 9.2

#### Thiết lập môi trường 3 máy ảo để cấu hình Replica Set:
- **Máy ảo 1 (Primary)**: IP `192.168.80.222`
- **Máy ảo 2 (Secondary)**: IP `192.168.80.223`
- **Máy ảo 3 (Secondary)**: IP `192.168.80.224`

Sử dụng công cụ **MobaXterm** với **SSH** để truy cập command line của từng máy ảo.

### b) Quá trình cài đặt  

#### 1. Tắt tường lửa và SElinux
*Mục tiêu:*
- Đảm bảo kết nối thông suốt giữa các node trong Replica Set (tắt `firewalld`).
- Tránh xung đột về quyền truy cập với các tệp và thư mục cần thiết (tắt `SELinux`).
- Giảm thiểu phức tạp trong quá trình cài đặt và cấu hình trong môi trường phát triển.


*Thực hiện:*
- **Dừng firewalld, kiểm tra lại trạng thái:**
   ```bash
   systemctl stop firewalld
   systemctl status firewalld
- **Dừng SELinux (tạm dừng ngăn chặn các hành động không hợp lệ, chỉ giám sát)**
   ```bash
   setenforce 0
#### 2. Thiết lập hostname và file hosts
*Mục tiêu:*
- Đặt hostname cho các máy ảo và cấu hình địa chỉ IP tương ứng trong file `/etc/hosts`.


*Thực hiện:*
- **Chỉnh host name trên 3 node:**
   - Đặt tên cho từng node để dễ dàng quản lý:
     ```bash
     hostnamectl set-hostname db01
     hostnamectl set-hostname db02
     hostnamectl set-hostname db03
     ```

- **Cấu hình file hosts:**
   - Thêm địa chỉ IP và hostname vào file `/etc/hosts`:
     ```bash
     vi /etc/hosts
     192.168.80.222 db01
     192.168.80.223 db02
     192.168.80.224 db03
     ```
#### 3. Cấu hình Kernel Parameters
 Mở tệp: `vi /etc/sysctl.conf` và cấu hình thông số: 

  ```bash
 	fs.file-max = 655350
 	vm.swappiness = 1
 	vm.overcommit_memory = 2
 	vm.overcommit_ratio = 100
 	vm.min_free_kbytes = 335555 
 ```
  Áp dụng thay đổi: 
  ```bash
  /sbin/sysctl -p
 ```
#### 4. Cấu hình giới hạn tài nguyên 

 Mở tệp:  `/etc/security/limits.conf`, nội dung tệp:

```bash
postgres   soft    nofile  655350
postgres   hard    nofile  655350
postgres   soft    memlock 30198989
postgres   hard    memlock 30198989
```

#### 5. Cấu hình mạng

Mở tệp: `vi /etc/sysconfig/network`,  nội dung:

```bash
NETWORKING=yes
HOSTNAME=db01 
NOZEROCONF=yes
```
#### 6. Cấu hình NTP đồng bộ thời gian của hệ thống từ xa

Chỉnh sửa tệp `/etc/chrony.conf`

```bash
server 192.168.80.222 iburst
server 192.168.80.223 iburst
server 192.168.80.224 iburst
```
Khởi động và kích hoạt dịch vụ `chronyd`

```bash
systemctl start chronyd
systemctl enable chronyd
```

#### 7. Tạo người dùng PostgreSQL

#### 3. Cài đặt hoặc cập nhật thư viện Glibc
*Mục tiêu:*
- Đảm bảo hệ thống có đầy đủ các thư viện cần thiết để thực hiện chạy các ứng dụng.

*Thực hiện:*
- **Cài đặt thư viện Glibc**
   ```bash
   yum install glibc -y
   ```
>Quá trình cài đặt thư viện có thể bị lỗi nếu không ping được tên miền `google.com` do cấu hình DNS không đúng.
>Kiểm tra file cấu hình DNS tại `/etc/resolv.conf` và đảm bảo nội dung trong file như sau:
>```bash
>nameserver 8.8.8.8
>nameserver 8.8.4.4
>```

#### 4. Cài đặt MongoDB

*Mục tiêu:*
- Cài đặt MongoDB phiên bản 7.0.14 trên cả 3 node thông qua repo.

*Thực hiện:*
- **Tạo file repo cho MongoDB:**
   - Tạo file `/etc/yum.repos.d/mongodb-enterprise-7.0.repo` với nội dung sau:
  
     ```bash
     [mongodb-enterprise-7.0]
     name=MongoDB Enterprise Repository
     baseurl=https://repo.mongodb.com/yum/redhat/7/mongodb-enterprise/7.0/$basearch/
     gpgcheck=1
     enabled=1
     gpgkey=https://pgp.mongodb.com/server-7.0.asc
     ```
- **Cài đặt MongoDB:**
   - Chạy lệnh sau để cài đặt MongoDB và các thành phần liên quan:
     ```bash
     yum install -y mongodb-enterprise-7.0.14
     ```

#### 5. Cấu hình hệ thống 

*Mục tiêu:*
- Cấu hình các tham số hệ thống và kernel cho MongoDB để tối ưu hóa hiệu suất.


*Thực hiện:*

 **a) Cấu hình `hugepages`:**
>Transparent Huge Pages (THP) là một tính năng quản lý bộ nhớ giúp cải thiện hiệu suất cho một số loại tải công việc, nhưng đối với MongoDB, nó có thể gây ra vấn đề với hiệu suất và độ trễ không mong muốn.
   - Tạo file cấu hình để vô hiệu hóa Transparent Huge Pages:
     ```bash
     vi /etc/systemd/system/disable-transparent-huge-pages.service
     ```
   - Nội dung file:
     ```bash
     [Unit]
     Description=Disable Transparent Huge Pages (THP)
     DefaultDependencies=no
     After=sysinit.target local-fs.target
     Before=mongod.service
     
     [Service]
     Type=oneshot
     ExecStart=/bin/sh -c 'echo never | tee /sys/kernel/mm/transparent_hugepage/enabled > /dev/null'
     
     [Install]
     WantedBy=basic.target
     ```
   - Khởi động và kích hoạt dịch vụ tắt THP:
     ```bash
     systemctl daemon-reload
     systemctl start disable-transparent-huge-pages
     systemctl enable disable-transparent-huge-pages
     ```

**b) Cấu hình tham số OS:**
>Các thông số này được cấu hình để tối ưu hóa hệ thống cho MongoDB bằng cách điều chỉnh cách hệ điều hành Linux quản lý bộ nhớ và kết nối mạng.
   - Chỉnh sửa file `/etc/sysctl.conf`:
     ```bash
     net.ipv4.ip_local_port_range = 1024 65530
     vm.max_map_count = 102400
     fs.file-max = 6815744
     kernel.pid_max = 64000
     kernel.threads-max = 64000
     vm.swappiness = 1
     net.ipv4.tcp_keepalive_time = 120
     ```

**c) Cấu hình tham số kernel cho MongoDB:**
>Đây là các thiết lập giới hạn tài nguyên cho người dùng mongod và root, nhằm đảm bảo rằng MongoDB có đủ tài nguyên (file descriptor, tiến trình, bộ nhớ) để hoạt động mà không gặp vấn đề khi tải nặng.
   - Chỉnh sửa file `/etc/security/limits.d/99-mongodb-nproc.conf`:

     ```bash
     mongod soft nofile 64000
     mongod hard nofile 64000
     root soft nofile 64000
     root hard nofile 640000
     mongod soft nproc 64000
     mongod hard nproc 64000
     ```

#### 6. Cấu hình MongoDB và Replica Set

*Mục tiêu:*
- Cấu hình 3 node để hoạt động như một Replica Set, với 1 Primary và 2 Secondary nodes.

*Thực hiện:*

**a) Tạo thư mục dữ liệu cho MongoDB và thay đổi quyền sở hữu:**
   - Trên cả 3 node:
     ```bash
     mkdir -p /data/datafile
     chown -R mongod:mongod /data
     ```

**b) Tạo keyfile trên node 1 và sao chép sang các node khác:**
   - Trên node 1:
     ```bash
     openssl rand -base64 756 > /data/mongo-keyfile
     chmod 400 /data/mongo-keyfile  #chỉ chủ sở hữu mới có quyền đọc file
     chown mongod:mongod /data/mongo-keyfile
     ```
   - Sao chép file `mongo-keyfile` sang node 2 và node 3, sau đó thiết lập quyền như trên.

      ```bash
      scp /data/mongo-keyfile user@192.168.80.223:/data/mongo-keyfile
      scp /data/mongo-keyfile user@192.168.80.224:/data/mongo-keyfile
      ```

**c) Cấu hình MongoDB trên từng node:**
   - **Node 1 (Primary)**:
     ```bash
     cat /etc/mongod.conf
     # Nội dung chính
     systemLog:
       destination: file
       path: /var/log/mongodb/mongod.log
     storage:
       dbPath: /data/datafile
     net:
       port: 27017
       bindIp: 0.0.0.0
     security:
       authorization: enabled
       keyFile: /data/mongo-keyfile
     replication:
       replSetName: "test"
     ```

   - **Node 2 (Secondary)** và **Node 3 (Secondary)**:
     - Cấu hình của node 1.


#### 7. Khởi tạo và quản lý Replica Set

*Mục tiêu:*
- Khởi tạo Replica Set và quản lý trạng thái của nó.

*Thực hiện:*

a) **Khởi động MongoDB trên cả 3 node:**
   
   ```bash
   systemctl start mongod
   ```
#### Cấu hình và khởi động MongoDB Replica Set (3 Node)


Trước tiên, khởi động MongoDB trên **Node 1**:

```bash
systemctl start mongod
```

b) Tạo user quản trị trên Node 1
Kết nối vào MongoDB shell bằng lệnh mongosh:

```bash
mongosh
```

Chuyển sang cơ sở dữ liệu admin và tạo user quản trị với quyền root:

```javascript
use admin
db.createUser({
  user: "mongodba",
  pwd: "123312##",
  roles: [{ role: "root", db: "admin" }]
})
```

c) Cấu hình lại file cấu hình trên Node 1
Sau khi tạo user, dừng MongoDB trên Node 1 để chỉnh sửa file cấu hình:

```bash
systemctl stop mongod
```

Chỉnh sửa file cấu hình /etc/mongod.conf để kích hoạt bảo mật và thiết lập Replica Set:

```bash
vi /etc/mongod.conf
```

Thêm các dòng sau vào file cấu hình:

```yaml
security:
  authorization: enabled
  keyFile: /data/mongo-keyfile

replication:
  replSetName: "test"
```

d) Khởi động lại MongoDB trên Node 1
Sau khi chỉnh sửa cấu hình, khởi động lại MongoDB trên Node 1:

```bash
systemctl start mongod
```

e) Khởi động MongoDB trên Node 2 và Node 3

f) Kết nối lại vào MongoDB trên Node 1 với user đã tạo
Kết nối vào MongoDB trên Node 1 bằng user mongodba đã tạo trước đó:

```bash
mongosh -u mongodba -p 123312## --authenticationDatabase admin
```

g) Khởi tạo Replica Set
Sau khi kết nối vào MongoDB, thực hiện khởi tạo Replica Set với lệnh rs.initiate():

```javascript
rs.initiate({
  _id: "test",
  members: [
    { _id: 0, host: "192.168.80.222:27017", priority: 3 },
    { _id: 1, host: "192.168.80.223:27017", priority: 2 },
    { _id: 2, host: "192.168.80.224:27017", priority: 1 }
  ]
})
```
