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
 Kỳ vọng: Tất cả các Secondary đều nhận được dữ liệu và có cùng trạng thái.

**Test Case 4**: Ghi dữ liệu trong Secondary
- Mô tả: Thực hiện ghi dữ liệu trực tiếp vào Secondary và xác nhận rằng việc này không thành công.
- Kỳ vọng: Ghi dữ liệu vào Secondary sẽ bị từ chối.



Chuỗi kết nối vào các công cụ thực hiện truy vấn của người dùng sẽ không bị thay đổi trong các trường hợp như failover. Để đảm bảo tính liên tục và tự động, khi Primary gặp sự cố và chuyển sang node khác, công cụ sẽ tự động xét các node trong cụm Replica và kết nối với ip của node Primary mới. Trường hợp chuỗi kết nối bị thay đổi là khi có thay đổi về số lượng các node như tăng hoặc giảm node trong cụm.
