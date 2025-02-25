## Quản trị Logical Volume Manager
>Quản trị Logical Volume Manager(LVM) là một cách linh hoạt để quản lý dung lượng ổ đĩa trong Linux, dễ dàng mở rộng, thu nhỏ và quản lý không gian lưu trữ động mà không ảnh hưởng đến dữ liệu.

### 1. Các thành phần trong LVM
   - Physical Volume (PV): Các ổ cứng hoặc phân vùng được sử dụng cho LVM.
   - Volume Group (VG): Tập hợp các PV để tạo thành một không gian lưu trữ chung.
   - Logical Volume (LV): Được tạo từ VG, hoạt động như một phân vùng ổ đĩa linh hoạt.
   - Physical Extent (PE): Đơn vị lưu trữ nhỏ nhất trong VG, thường có kích thước 4MB hoặc 8MB.
### 2. Các thao tác trên LVM

#### A. Tạo mới Logical Volume
- Bước 1: Kiểm tra ổ đĩa trống
  - Chạy lệnh để xem danh sách các ổ đĩa chưa được sử dụng:
    ```bash
      lsblk
    ```
  - Hoặc kiểm tra các phân vùng:

    ```bash
    fdisk -l
    ```

- Bước 2: Tạo Physical Volume (PV)
  - Chọn ổ đĩa hoặc phân vùng cần dùng cho LVM, ví dụ /dev/sdb:
    ```bash
    pvcreate /dev/sdb
    ```
  - Kiểm tra PV đã tạo:
    ```bash
    pvdisplay
    ```
- Bước 3: Tạo Volume Group (VG)
  - Tạo một Volume Group có tên vg_data:
    ```bash
    vgcreate vg_data /dev/sdb
    ```
  - Kiểm tra thông tin VG:
    ```bash
      vgdisplay
    ```
- Bước 4: Tạo Logical Volume (LV)
  - Tạo một Logical Volume lv_storage dung lượng 10GB:
    ```bash
    lvcreate -L 10G -n lv_storage vg_data
    ```
  - Hoặc dùng toàn bộ dung lượng còn lại của VG:
    ```bash
      lvcreate -l 100%FREE -n lv_storage vg_data
    ```
  - Kiểm tra LV đã tạo:
    ```bash
    lvdisplay
    ```
- Bước 5: Định dạng và gắn kết LV
  - Định dạng hệ thống tập tin (ví dụ: ext4):
    ```bash
      mkfs.ext4 /dev/vg_data/lv_storage
    ```
  - Tạo thư mục để mount:
    ```bash
      mkdir /mnt/lvm_storage
    ```
  - Gắn LV vào thư mục:
    ```bash
    mount /dev/vg_data/lv_storage /mnt/lvm_storage
    ```
  - Thêm vào /etc/fstab để tự động mount khi khởi động:
    ```bash
    echo "/dev/vg_data/lv_storage /mnt/lvm_storage ext4 defaults 0 0" >> /etc/fstab
    ```
#### B. Mở rộng Logical Volume
- Bước 1: Kiểm tra dung lượng còn lại
    ```bash
      vgdisplay vg_data
    ```
- Bước 2: Mở rộng Logical Volume
  - Ví dụ: tăng thêm 5GB vào lv_storage:
    ```bash
    lvextend -L +5G /dev/vg_data/lv_storage
    ```
  - Hoặc dùng toàn bộ dung lượng còn lại:
    ```bash
    lvextend -l +100%FREE /dev/vg_data/lv_storage
    ```
- Bước 3: Cập nhật hệ thống tập tin
  - Nếu là ext4, chạy:
    ```bash
      resize2fs /dev/vg_data/lv_storage
    ```
  - Nếu là XFS, chạy:
    ```bash
    xfs_growfs /mnt/lvm_storage
    ```
#### C. Thu nhỏ Logical Volume
>Việc thu nhỏ có thể làm mất dữ liệu nếu không cẩn thận. Luôn backup dữ liệu trước khi thực hiện.

- Bước 1: Kiểm tra hệ thống tập tin
  - Nếu là ext4, kiểm tra lỗi trước khi thu nhỏ:
    ```bash
    e2fsck -f /dev/vg_data/lv_storage
    ```
- Bước 2: Thu nhỏ hệ thống tập tin
  - Ví dụ: giảm dung lượng về 10GB
    ```bash
    resize2fs /dev/vg_data/lv_storage 10G
    ```
- Bước 3: Thu nhỏ Logical Volume
    ```bash
    lvreduce -L 10G /dev/vg_data/lv_storage
    ```
- Bước 4: Kiểm tra lại
    ```bash
    lvdisplay
    ```
#### D. Xóa Logical Volume
- Bước 1: Gỡ bỏ LV
```bash
umount /mnt/lvm_storage
```
- Bước 2: Xóa LV
```bash
lvremove /dev/vg_data/lv_storage
```
- Bước 3: Xóa Volume Group (nếu không cần nữa)
```bash
vgremove vg_data
```
- Bước 4: Xóa Physical Volume
```bash
pvremove /dev/sdb
```
### 3. Kiểm tra lại LVM
- Kiểm tra các PV:
```bash
pvs
```
- Kiểm tra các VG:
```bash
vgs
```
- Kiểm tra các LV:
```bash
lvs
```
