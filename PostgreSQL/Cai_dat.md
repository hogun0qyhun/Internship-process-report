## Cài đặt PostgreSql và Replica Set
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

### b) Quá trình cài đặt

- Cấu hình tệp `/etc/hosts` trên cả hai node:

``` bash
vi /etc/hosts

# Node 1
192.168.80.222  db01

# Node 2
192.168.80.223  db02

```
- Đặt hostname cho cả hai node:

``` bash
hostnamectl set-hostname db01  # trên node 1
systemctl restart systemd-hostnamed

hostnamectl set-hostname db02  # trên node 2
systemctl restart systemd-hostnamed

```

-  Cấu hình `/etc/sysctl.conf` trên node 1:

```bash
vi /etc/sysctl.conf

fs.file-max = 655350
vm.swappiness = 1
vm.overcommit_memory = 2
vm.overcommit_ratio = 100
vm.min_free_kbytes = 335555

/sbin/sysctl -p

```

- 










































