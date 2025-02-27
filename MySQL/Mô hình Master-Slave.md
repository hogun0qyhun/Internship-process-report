### MySQL Replication
>MySql replication là một quá trình cho phép dữ liệu từ một server MySQL (master) được sao lưu lại trên một hoặc nhiều server MySQL khác (slaves). Nó thường được sử dụng để cấp quyền đọc cho nhiều server khác cho khả năng nhân rộng, đồng thời nó cũng được sử dụng cho mục đích khác như là để dự phòng lỗi cho server master, hoặc là để phân tích dữ liệu trên server slave giúp giảm tải cho server master.

 ### *Cấu hình*

- **Master**: 14.225.69.27
  - Tạo thư mục: nếu chưa có
    ```bash
    sudo mkdir -p /var/log/mysql
    sudo chown -R mysql:mysql /var/log/mysql
    sudo chmod 750 /var/log/mysql
    ```
  - Chỉnh sửa /etc/my.conf
    ```bash
    log_bin = /var/log/mysql/mysql-bin.log
    server_id = 1
    bind-address = 0.0.0.0 
    ```
  - Khởi động lại dịch vụ
    ```bash
    systemctl restart mysql
    ```
  - Đăng nhập tạo tài khoản replication

    ```bash
    mysql -u root -p
    
    CREATE USER 'replication_user'@'%' IDENTIFIED BY '123123aA@';
    GRANT REPLICATION SLAVE ON *.* TO 'replication_user'@'%';
    FLUSH PRIVILEGES;
    
    - Kiểm tra trạng thái của Master:
    SHOW STATUS;
    ```

- **Slave**: 14.225.69.51

  - Chỉnh sửa /etc/my.conf
    ```bash
    server_id = 2
    bind-address = 0.0.0.0
    read_only = 1
    super_read_only = 1
    replicate_wild_ignore_table=mysql.%
    ```
  - Khởi động lại dịch vụ, đăng nhập
  
    ```bash
    systemctl restart mysql
    mysql -u root -p
    ```
  - Thiết lập Slave sao chép từ Master
      ```bash
      CHANGE REPLICATION SOURCE TO 
          SOURCE_HOST='14.225.69.27',
          SOURCE_USER='replication_user',
          SOURCE_PASSWORD='123123aA@',
          SOURCE_LOG_FILE='mysql-bin.000003',
          SOURCE_LOG_POS=2833;
          START REPLICA;
          
          RESET REPLICA ALL;
          
          STOP REPLICA;
          RESET REPLICA;
          SHOW REPLICA STATUS\G;
      ```
 ### *Kết quả*
- Tại node Slave, đăng nhập hiển thị các database đang có
  ```bash
    [root@labtest-1-217202583118am ~]# ip a
    1: lo: <LOOPBACK,UP,LOWER_UP> mtu 65536 qdisc noqueue state UNKNOWN group default qlen 1000
        link/loopback 00:00:00:00:00:00 brd 00:00:00:00:00:00
        inet 127.0.0.1/8 scope host lo
           valid_lft forever preferred_lft forever
        inet6 ::1/128 scope host
           valid_lft forever preferred_lft forever
    2: eth0: <BROADCAST,MULTICAST,UP,LOWER_UP> mtu 1500 qdisc pfifo_fast state UP group default qlen 1000
        link/ether fa:16:3e:ce:66:58 brd ff:ff:ff:ff:ff:ff
        inet 192.168.0.35/24 brd 192.168.0.255 scope global dynamic eth0
           valid_lft 73607sec preferred_lft 73607sec
        inet6 fe80::f816:3eff:fece:6658/64 scope link
           valid_lft forever preferred_lft forever
    3: eth1: <BROADCAST,MULTICAST,UP,LOWER_UP> mtu 1500 qdisc pfifo_fast state UP group default qlen 1000
        link/ether fa:16:3e:72:cf:1d brd ff:ff:ff:ff:ff:ff
        inet 14.225.69.51/24 brd 14.225.69.255 scope global dynamic eth1
           valid_lft 85299sec preferred_lft 85299sec
        inet6 fe80::f816:3eff:fe72:cf1d/64 scope link
           valid_lft forever preferred_lft forever
    [root@labtest-1-217202583118am ~]# mysql -u root -p
    Enter password:
    Welcome to the MySQL monitor.  Commands end with ; or \g.
    Your MySQL connection id is 640
    Server version: 9.2.0 MySQL Community Server - GPL
    
    Copyright (c) 2000, 2025, Oracle and/or its affiliates.
    
    Oracle is a registered trademark of Oracle Corporation and/or its
    affiliates. Other names may be trademarks of their respective
    owners.
    
    Type 'help;' or '\h' for help. Type '\c' to clear the current input statement.
    
    mysql> show databases;
    +--------------------+
    | Database           |
    +--------------------+
    | information_schema |
    | mysql              |
    | performance_schema |
    | sys                |
    | test               |
    | test1              |
    | testReplica        |
    +--------------------+
    7 rows in set (0.00 sec)

  ```
  
 - Tại node Master, ghi dữ liệu mới
  ```bash
      [root@mysql ~]# ip a
      1: lo: <LOOPBACK,UP,LOWER_UP> mtu 65536 qdisc noqueue state UNKNOWN group default qlen 1000
          link/loopback 00:00:00:00:00:00 brd 00:00:00:00:00:00
          inet 127.0.0.1/8 scope host lo
             valid_lft forever preferred_lft forever
          inet6 ::1/128 scope host
             valid_lft forever preferred_lft forever
      2: eth0: <BROADCAST,MULTICAST,UP,LOWER_UP> mtu 1500 qdisc pfifo_fast state UP group default qlen 1000
          link/ether fa:16:3e:76:32:78 brd ff:ff:ff:ff:ff:ff
          inet 192.168.0.202/24 brd 192.168.0.255 scope global dynamic eth0
             valid_lft 82152sec preferred_lft 82152sec
          inet6 fe80::f816:3eff:fe76:3278/64 scope link
             valid_lft forever preferred_lft forever
      3: eth1: <BROADCAST,MULTICAST,UP,LOWER_UP> mtu 1500 qdisc pfifo_fast state UP group default qlen 1000
          link/ether fa:16:3e:65:99:75 brd ff:ff:ff:ff:ff:ff
          inet 14.225.69.27/24 brd 14.225.69.255 scope global dynamic eth1
             valid_lft 63596sec preferred_lft 63596sec
          inet6 fe80::f816:3eff:fe65:9975/64 scope link
             valid_lft forever preferred_lft forever
      [root@mysql ~]# mysql -u root -p
      Enter password:
      Welcome to the MySQL monitor.  Commands end with ; or \g.
      Your MySQL connection id is 2387
      Server version: 9.2.0 MySQL Community Server - GPL
      
      Copyright (c) 2000, 2025, Oracle and/or its affiliates.
      
      Oracle is a registered trademark of Oracle Corporation and/or its
      affiliates. Other names may be trademarks of their respective
      owners.
      
      Type 'help;' or '\h' for help. Type '\c' to clear the current input statement.
      
      mysql> create database quanlythietbi;
      Query OK, 1 row affected (0.01 sec)
  
  ```
- Node Slave đã sao chép được dữ liệu mới ghi vào
  ```bash
    mysql> show databases;
    +--------------------+
    | Database           |
    +--------------------+
    | information_schema |
    | mysql              |
    | performance_schema |
    | quanlythietbi      |
    | sys                |
    | test               |
    | test1              |
    | testReplica        |
    +--------------------+
    8 rows in set (0.00 sec)
   ```
___ 
### Cấu hình tự động kill session chạy lâu (sử dụng:  Bash Script + Crontab)

- Tạo file auto_kill_sessions.sh
 vi /usr/local/bin/auto_kill_sessions.sh

    ```bash
		# Thông tin kết nối MySQL
		DB_USER="root"
		DB_PASS="123123aA@"      
		HOST="localhost"            # Địa chỉ MySQL Server (localhost hoặc IP)

		# Giới hạn thời gian tối đa cho phép (giây)
		THRESHOLD=30               # Session chạy quá 30 giây sẽ bị kill

		# Lấy danh sách các session chạy lâu hơn $THRESHOLD
		SESSIONS=$(mysql -u $DB_USER -p$DB_PASS -h $HOST -e "
			SELECT id 
			FROM information_schema.processlist 
			WHERE Command='Query' AND Time > $THRESHOLD;" -s --skip-column-names)

		# Kiểm tra và kill từng session
		for ID in $SESSIONS; do
			echo "$(date +"%Y-%m-%d %H:%M:%S") - Đang kill session ID: $ID" >> /var/log/mysql_kill_sessions.log
			mysql -u $DB_USER -p$DB_PASS -h $HOST -e "KILL $ID;"
		done

		# Thông báo hoàn thành
		echo "$(date +"%Y-%m-%d %H:%M:%S") - ✅ Đã kill xong các session chạy quá lâu!" >> /var/log/mysql_kill_sessions.log
    ```
- Cấp quyền thực thi:
  ```bash
  	chmod +x /usr/local/bin/auto_kill_sessions.sh
  ```

- Thiết Lập Chạy Tự Động với Crontab
	- Mở Crontab: `crontab -e`
	- Thêm dòng sau để script chạy mỗi 5 phút: 
	```bash
 		*/5 * * * * /usr/local/bin/auto_kill_sessions.sh >> /var/log/mysql_kill_sessions.log 2>&1
	```
	- Xem danh sách cronjob đã thiết lập: `crontab -l`

	- Kiểm tra các session chạy lâu:
	```bash
		mysql -u root -p -e "
			SELECT id, user, host, db, command, time, state, info
			FROM information_schema.processlist
			WHERE Command NOT IN ('Binlog Dump', 'Replica', 'Connect', 'Daemon')
			AND user NOT IN ('event_scheduler')
			AND Time > 30;" 
	```		
- Xem các session: `mysql -u root -p -e "SHOW PROCESSLIST;"`
- Xem lịch sử đã kill: `cat /var/log/mysql_kill_sessions.log`
