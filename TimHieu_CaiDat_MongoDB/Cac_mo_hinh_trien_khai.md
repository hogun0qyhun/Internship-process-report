## Các Mô Hình Triển Khai và Quản Lý Hệ Thống Của MongoDB

### a) Mô Hình Standalone
Đây là mô hình đơn giản nhất, chỉ sử dụng một instance MongoDB duy nhất.

- **Ưu điểm**: Dễ cài đặt, quản lý.
- **Nhược điểm**: Không có dự phòng (fault tolerance) và không thể mở rộng (scalability). Phù hợp cho các ứng dụng nhỏ hoặc trong môi trường phát triển (development).

---

### b) Mô Hình Replica Set
Một **Replica Set** bao gồm một **Primary Node** và nhiều **Secondary Nodes**. Primary xử lý các thao tác ghi và đồng bộ với các Secondary thông qua **Oplog**.

- **Primary**: Chịu trách nhiệm xử lý mọi yêu cầu ghi và đồng bộ với các Secondary.
- **Secondary**: Sao chép dữ liệu từ Primary và có thể tham gia xử lý yêu cầu đọc.
- **Arbiter**: Chỉ tham gia bỏ phiếu để giúp lựa chọn Primary mới khi cần, nhưng không lưu trữ dữ liệu.

#### Ưu điểm:
- Cung cấp tính sẵn sàng cao (high availability), khi Primary gặp sự cố, hệ thống sẽ tự động chuyển một Secondary thành Primary.

#### Nhược điểm:
- Tăng chi phí hạ tầng vì cần nhiều máy chủ.
- Khi dữ liệu quá lớn, Replica Set không đủ khả năng mở rộng lưu trữ.
- Giới hạn khả năng ghi trên node Primary, giảm hiệu suất ghi khi hệ thống phải xử lý nhiều thao tác đồng thời.
  
Phù hợp cho các ứng dụng cần tính dự phòng và đảm bảo tính sẵn sàng cao.

---

### c) Mô Hình Sharding
**Sharding** là phương pháp chia nhỏ dữ liệu trên nhiều máy chủ (**shards**) để tăng cường khả năng mở rộng theo chiều ngang (**horizontal scaling**).

Một **Sharded Cluster** bao gồm:

- **Shard**: Lưu trữ một phần dữ liệu của hệ thống.
- **Config Server**: Lưu thông tin về metadata và cấu hình của cluster.
- **Mongos Router**: Làm nhiệm vụ định tuyến truy vấn đến đúng shard.

#### Ưu điểm:
- Cung cấp khả năng mở rộng lớn cho các hệ thống với lượng dữ liệu cực lớn và lưu trữ phân tán.

#### Nhược điểm:
- Triển khai phức tạp và yêu cầu quản lý cẩn thận.

Phù hợp với các ứng dụng có khối lượng dữ liệu lớn và yêu cầu mở rộng linh hoạt.

---

### d) Mô Hình Hybrid
Đây là mô hình kết hợp giữa **Replica Set** và **Sharding**. Mỗi shard trong Sharded Cluster có thể là một Replica Set để vừa đảm bảo tính mở rộng, vừa có dự phòng.

#### Ưu điểm:
- Cung cấp cả khả năng mở rộng và dự phòng.

#### Nhược điểm:
- Cấu hình phức tạp, đòi hỏi chi phí cao để duy trì nhiều máy chủ. Phù hợp với các ứng dụng lớn, đòi hỏi tính sẵn sàng và khả năng mở rộng cực cao.
---
### e) So sánh các mô hình triển khai 
|  | Standalone | Replica Set | Sharded Cluster | Hybrid (Replica Set + Sharded Cluster) |
| --- | --- | --- | --- | --- |
| *Số lượng máy chủ* | 1 | 3 hoặc nhiều hơn | 2 hoặc nhiều shard, mỗi shard ít nhất 1 máy | 2 hoặc   nhiều shard, mỗi shard là một Replica Set |
| *Tính năng* | - Chỉ có một instance MongoDB | - Sao chép dữ liệu giữa các node, đảm bảo tính sẵn sàng và dự phòng | - Phân mảnh dữ liệu trên nhiều server để mở rộng | - Phân mảnh dữ liệu và sao chép để đảm bảo tính sẵn sàng, dự phòng và mở rộng |
| *Mở rộng theo chiều ngang* | Không | Có giới hạn | Cao | Rất cao |
| *Tính dự phòng (Failover)* | Không | Có (nếu Primary gặp sự cố , một Secondary sẽ được bầu làm Primary) | Không (tuy nhiên, các shard có thể được cấu hình với Replica Set để có dự phòng) | Rất cao (Sharding kết hợp Replica Set) |
| *Khả năng chịu lỗi* | Không | Cao (do sao chép dữ liệu giữa các node) | Trung bình (nếu một shard gặp sự cố, chỉ mất dữ liệu của shard đó) | Rất cao (dữ liệu được sao chép trong từng shard) |
| *Hiệu suất truy vấn* | Cao với dữ liệu nhỏ | Trung bình đến cao (có thể dùng Secondary để cân bằng tải cho đọc) | Cao với dữ liệu lớn nhờ phân mảnh dữ liệu | Rất cao, vừa mở rộng vừa đảm bảo tính dự phòng |
| *Sử dụng cho môi trường* | Phát triển thử nghiệm | Sản xuất với yêu cầu sẵn sàng cao | Ứng dụng có lượng dữ liệu lớn và cần mở rộng theo chiều ngang | Ứng dụng có quy mô lớn, yêu cầu cao về cả mở rộng và dự phòng |
| *Quản lý phức tạp* | Đơn giản | Trung bình | Phức tạp (cần định cấu hình sharding và quản lý nhiều shard) | Rất phức tạp (kết hợp quản lý sharding và replica set) |
| *Yêu cầu phần cứng* | Thấp | Trung bình | Cao | Rất cao |

