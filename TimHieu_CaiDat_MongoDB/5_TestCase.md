## Các trường hợp xảy ra trong quá trình vận hành Replica Set
### Node trở thành Primary
 *Khi một node trong Replica Set trở thành primary, nó sẽ nhận các ghi (write) và phân phối các thay đổi đến các secondary nodes. Nếu primary node gặp sự cố hoặc ngừng hoạt động, một trong các secondary nodes sẽ tự động được bầu chọn làm primary mới.*
 
**Test Case 1**: Chuyển đổi Primary
- Mô tả: Thực hiện tắt máy chủ Primary và xác nhận rằng một trong các Secondary trở thành Primary mới.
- Kỳ vọng: Một Secondary trở thành Primary mà không mất dữ liệu.
  
**Test Case 2**: Khôi phục Primary
- Mô tả: Khởi động lại máy chủ Primary và kiểm tra xem nó có trở thành Primary hay không.
- Kỳ vọng: Primary mới khôi phục sẽ tự động trở thành Secondary, trong trường hợp thứ tự ưu tiên của node cao hơn các node còn lại, node đó sẽ tự động trở thành Primary.

**Test Case 3**: Ghi dữ liệu trong Primary
- Mô tả: Ghi một số dữ liệu vào Primary và xác nhận rằng dữ liệu được đồng bộ hóa sang các Secondary.
- Kỳ vọng: Tất cả các Secondary đều nhận được dữ liệu và có cùng trạng thái.

**Test Case 4**: Ghi dữ liệu trong Secondary
- Mô tả: Thực hiện ghi dữ liệu trực tiếp vào Secondary và xác nhận rằng việc này không thành công.
- Kỳ vọng: Ghi dữ liệu vào Secondary sẽ bị từ chối.

### Node bị mất kết nối

*Một node có thể mất kết nối với Replica Set do mạng không ổn định hoặc do node đó bị tắt. Khi một node bị mất kết nối, nó sẽ không nhận được các cập nhật từ primary và sẽ trở thành RECOVERING hoặc ROLLBACK khi nó quay trở lại.*

**Xử lý:**
- Kiểm tra cấu hình mạng và trạng thái của node.
- Khi node phục hồi, MongoDB sẽ tự động đồng bộ hóa dữ liệu từ primary.

### Secondary node không bắt kịp
*Một secondary node có thể không theo kịp với primary do tải quá nặng hoặc không đủ tài nguyên (CPU, RAM, I/O). Nếu một secondary node không cập nhật kịp thời, nó có thể không đủ điều kiện để bầu chọn làm primary.*

**Xử lý:**
- Giám sát hiệu suất của secondary nodes và đảm bảo rằng chúng có đủ tài nguyên.
- Kiểm tra logs để tìm hiểu nguyên nhân và điều chỉnh cấu hình nếu cần.

### Rollback
*Nếu một secondary node bị tắt và sau đó quay lại, nó có thể gặp tình trạng rollback nếu nó có dữ liệu không khớp với primary. MongoDB sẽ khôi phục dữ liệu này về trạng thái nhất quán với primary.*

**Xử lý:**
- Theo dõi trạng thái của node bằng lệnh rs.status().
- Đảm bảo rằng các secondary nodes có thể phục hồi về trạng thái chính xác mà không mất dữ liệu quan trọng.

### Mất dữ liệu
*Nếu có sự cố xảy ra với primary node và các ghi chưa được sao chép vào secondary nodes, có thể dẫn đến mất dữ liệu.*

**Xử lý:**
- Sử dụng backup thường xuyên để đảm bảo dữ liệu không bị mất.
- Cân nhắc sử dụng write concern cao hơn để đảm bảo rằng dữ liệu đã được sao chép đến ít nhất một node khác trước khi xác nhận ghi.

#### Chuỗi kết nối (connection string) của các công cụ thực hiện truy vấn có thay đổi khi failover xảy ra hay không.

>Chuỗi kết nối vào các công cụ thực hiện truy vấn của client sẽ không bị thay đổi trong các trường hợp như failover. Để đảm bảo tính liên tục và tự động, khi Primary gặp sự cố và chuyển sang node khác, công cụ sẽ tự động xét các node trong cụm Replica và kết nối với IP của node Primary mới. Trường hợp chuỗi kết nối bị thay đổi là khi có thay đổi về số lượng các node như tăng hoặc giảm node trong cụm.
