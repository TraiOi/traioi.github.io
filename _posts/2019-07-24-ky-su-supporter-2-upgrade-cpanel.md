---
layout: post
title:  "Ký sự Supporter (Phần 2): Upgrade Cpanel"
date:   2019-07-24 21:02:03 +0700
categories: cty1
description: Lần đầu được upgrade Cpanel.
---
<link rel="stylesheet" href="{{ "/assets/posts.css" | relative_url }}">

Hôm nay vừa mới ngủ dậy thì nhận được tin nhắn của sếp nhờ 1 trường hợp đặc biệt là upgrade hệ thống Cpanel và các service bên trong cho khách vì hệ thống hiện tại của khách đã cũ.  

Upgrade Cpanel đối với tôi thì cũng không có gì đặc biệt, "run tay" ở chỗ khách này là khách lớn, có thuê cả gói quản trị bên mình nữa nên lỡ có làm sai gì thì đền ốm.  

Khách hẹn tiếp 22h bắt đầu upgrade nên tôi phải vào review sơ qua trước các cấu hình hiện tại để lập kế hoạch trước khi vào "chiến đấu".  

### 1. Review cấu hình

Cpanel hiện tại của khách đang chạy Cent 6.10 và đang dùng bản v76 như này.  

![img](/assets/img/cty1-2-2.png)  

Chưa gì mới vào đã thấy thông báo như hình rồi.  

![img](/assets/img/cty1-2-1.png)  

Cụ thể:
* Ở **bảng thông báo thứ 1** báo rằng Cpanel ở bản hiện tại đang sử dụng EasyApache 3 nhưng mà bản này đã hết date từ 31/12/2018 rồi. Nếu nâng cấp lên phiên bản Cpanel mới hơn bắt buộc phải dùng EasyApache 4, nếu không thì không upgrade được.  
* Ở **bảng thông báo thứ 2** thì báo rằng sắp tới CentOS 6 sẽ không còn được Cpanel hỗ trợ nữa, nhưng mà đến năm 2020 lận nên phần này tôi sẽ bỏ qua, năm sau đề nghị khách bảo trì phát nữa để nâng cấp lên CentOS 7 là được, hehe.  

May mắn do phiên bản Cpanel của khách cũng khá mới nên kiểm tra qua lại cũng chỉ có 1 số lưu ý vậy (đỡ run tay hơn 1 chút rồi).

### 2. Upgrade

#### 2.1. EasyApache
Bắt đầu trước với EasyApache 3 lên 4 trước nào. Nhưng mà search Google trước cho chắc ăn, xem còn lưu ý gì khác không.

May mắn là Cpanel cũng có sẵn luôn hướng dẫn riêng cho cái này luôn, còn chờ gì nữa mà chơi thôi ([How to Install or Uninstall EasyApache 4](https://documentation.cpanel.net/display/EA4/How+to+Install+or+Uninstall+EasyApache+4)).  

Ở đây họ hướng dẫn bằng 2 cách là qua giao diện và qua command line.  

![img](/assets/img/cty1-2-3.png)  

Với 1 người thích command line như tôi thì quá tuyệt vời rồi, SSH vào con server và chạy command thôi. Chạy command thì lại có thêm hướng dẫn nữa, đúng là có hỗ trợ tận răng thì làm gì cũng dễ dàng.  

![img](/assets/img/cty1-2-4.png)  

```
# /scripts/migrate_ea3_to_ea4 --run
```

10 phút sau ...

May mắn thay là chạy 1 lèo không lỗi lầm gì, ngoài ra còn ghi lại log cho mình xem nữa chứ, đồ trả phí có khác.  

![img](/assets/img/cty1-2-5.png)  

#### 2.2. Cpanel

Bây giờ tới "món chính" của "bữa tiệc": upgrade Cpanel. Tất nhiên là cũng có document giống EasyApache rồi ([Upgrade to Latest Version](https://documentation.cpanel.net/display/68Docs/Upgrade+to+Latest+Version)). Và cũng lại có 2 phương án là qua giao diện hoặc command line.  

![img](/assets/img/cty1-2-6.png)  

Quất thôi :v.  
```
# /scripts/upcp
```

1 tiếng sau ... (lâu vãi ra)

Lại chạy 1 lèo không lỗi lầm gì nhưng lại xuất hiện thêm yêu cầu upgrade MySQL nữa. Ngần ngại gì mà không tiếp tục chứ nhỉ, khách báo là upgrade toàn bộ service nếu có thể cơ mà.  

![img](/assets/img/cty1-2-7.png)  

#### 2.3. MySQL/MariaDB

Lúc này khi vào lại giao diện thì xuất hiện thêm thông báo mới.  

![img](/assets/img/cty1-2-8.png)  

Document lúc này có vẻ dài hơn các bước trên, ngoài ra **Warnings**  cũng dài hơn ([MySQL or MariaDB Upgrade](https://documentation.cpanel.net/display/82Docs/MySQL+or+MariaDB+Upgrade)). Liên quan đến database nên đành phải cẩn thận hơn thôi >"< (đống này mới gọi là đền ốm thật sự nếu lỗi nè).  

![img](/assets/img/cty1-2-9.png)  

##### 2.3.1. Warnings

Sau khi xem sơ qua document của Cpanel, tôi sẽ liệt kê ra một số cảnh báo của Cpanel phù hợp với Server mà tôi đang thao tác:

* Backup databases (phần này thì tất nhiên rồi).
* Upgrade xong thì không thể downgrade được nữa.
* MySQL không thể upgrade thành MariaDB phiên bản cao hơn và ngược lại.
* Nếu chuyển từ MySQL thành MariaDB thì không thể chuyển về lại MySQL được nữa.  
* `phpinfo` file có thể sẽ hiển thị phiên bản khác với MySQL mà Cpanel sử dụng.

Từ các cảnh báo trên thì tôi rút ra được 1 số điều cần làm trước khi thao tác upgrade:
* Backup databases.
* Kiểm tra xem Cpanel hiện tại đang dùng MySQL hay MariaDB.
* Sau khi upgrade xong thì kiểm tra lại phiên bản MySQL trong `phpinfo`.  

##### 2.3.2. Backup databases

Với 1 người thích kiểu "1 dòng lệnh mà làm được tất cả" như tôi thì sử dụng từng dòng `mysqldump` cho mỗi database thì chưa đủ, phải tăng độ khó cho bản thân chứ. Và đây là thành quả (những database hay string nào dư thừa thì nó sẽ báo lỗi nên cũng không ảnh hưởng gì nhiều đâu).  
```
# for db in $(mysql -e "show databases;"); do mysqldump $db > $db.sql; done
```

Và đây là lỗi >"<.  

![img](/assets/img/cty1-2-10.png)  

Nhưng sau khi research chút đỉnh thì lỗi này là do không đủ dung lượng trên server. Đành phải backup về máy cá nhân vậy.  
```
# for db in $(mysql -h [IP_Server] -u root -p [password_root_MySQL] -e "show databases;"); do mysqldump -h [IP_Server] -u root -p [password_root_MySQL] $db > $db.sql; done
```

1 tiếng sau ... Cái list databases hơn 9GB, haizzzz.  
```
[ traioi ]# du -sh db_bak/
9.3G	db_bak/
```

##### 2.3.3. Kiểm tra phiên bản MySQL/MariaDB

1h sáng rồi, mắt cũng mở hết lên luôn, nhưng cũng phải ráng làm cho xong, cứ nghĩ là nhanh lắm mà lòi ra tùm lum vấn đề.  

```
# mysql
Welcome to the MySQL monitor.  Commands end with ; or \g.
Your MySQL connection id is 2700
Server version: 5.5.62-cll MySQL Community Server (GPL)
...
```

Server dùng **MySQL** và phiên bản **5.5.62**, quá dễ.

##### 2.3.4. Upgrade

Bắt đầu upgrade MySQL thôi.  

Cpanel có hướng dẫn thì không tìm ra cách upgrade bằng command line nên chỉ còn cách sử dụng giao diện thôi, dù gì mắt cũng mờ rồi nên dùng giao diện cho dễ an toàn dễ thở, hic.  

> **Home »SQL Services »MySQL/MariaDB Upgrade**

* **Step 1/7:** Quất luôn cái mới nhất chứ còn đợi chờ gì nữa.  

![img](/assets/img/cty1-2-11.png)  

**Step 2/7: Upgrade Warnings**. Nhìn đáng sợ quá nhưng mà buồn ngủ quá nên tick hết cho lẹ vậy, chứ giờ có run tay cũng phải tick.  

![img](/assets/img/cty1-2-12.png)  

**Step 3/7: Upgrade Type**. Tới đây lại có thêm 2 options (sao mà cảm giác giống như mình đang vượt qua các dungeon đế đến cứu công chúa thế nhỉ, level nào cũng căng não >"<").
* **Unattended Upgrade:** Option này hệ thống sẽ tự động upgrade và rebuild lại PHP và Apache cho mình, mình chỉ việc ngồi chờ mà thôi.  
* **Interactive Upgrade:** Option này thì thì an toàn hơn, tôi có thể theo dõi và tự lựa chọn cấu hình phù hợp trong quá trình upgrade. (chọn option này)

![img](/assets/img/cty1-2-13.png)

**Step 4/7:** Sau 1 hồi chạy thì ... lỗi luôn.  

![img](/assets/img/cty1-2-14.png)

Theo như log cảnh báo thì lỗi này liên quan đến `.my.cnf`, và kiểm tra kỹ log thì lỗi báo rằng không thấy các biến ở dưới (có lẽ do bản mới bỏ các biến này).  
```
# tail -100 /var/log/mysqld.log
...
2019-07-24T18:50:12.412910Z 0 [ERROR] unknown variable 'key_buffer=128M'
...
2019-07-24T18:51:48.330618Z 0 [ERROR] unknown variable 'thread_concurrency=16'
..
```

Sau khi comment lại các biến đó thì thì start lên OK ngay.
```
# /etc/init.d/mysqld start
Starting mysqld:                                           [  OK  ]
```

Ở giao diện, chọn **Retry** để script chạy lại mà còn qua bước tiếp theo chứ.  

...! 2h sáng và xuất hiện thêm 1 nùi lỗi mới nữa ![:(](/assets/img/fb-frown.png).  
```
# tail -100 /var/log/mysqld.log
...
2019-07-24T19:28:13.371598Z 0 [ERROR] Native table 'performance_schema'.'replication_applier_status_by_coordinator' has the wrong structure
2019-07-24T19:28:13.371631Z 0 [ERROR] Native table 'performance_schema'.'replication_applier_status_by_worker' has the wrong structure
2019-07-24T19:28:13.371664Z 0 [ERROR] Native table 'performance_schema'.'replication_group_member_stats' has the wrong structure
2019-07-24T19:28:13.371708Z 0 [ERROR] Native table 'performance_schema'.'prepared_statements_instances' has the wrong structure
2019-07-24T19:28:13.371744Z 0 [ERROR] Native table 'performance_schema'.'user_variables_by_thread' has the wrong structure
2019-07-24T19:28:13.371775Z 0 [ERROR] Native table 'performance_schema'.'status_by_account' has the wrong structure
2019-07-24T19:28:13.371806Z 0 [ERROR] Native table 'performance_schema'.'status_by_host' has the wrong structure
2019-07-24T19:28:13.371837Z 0 [ERROR] Native table 'performance_schema'.'status_by_thread' has the wrong structure
2019-07-24T19:28:13.371869Z 0 [ERROR] Native table 'performance_schema'.'status_by_user' has the wrong structure
2019-07-24T19:28:13.371906Z 0 [ERROR] Native table 'performance_schema'.'global_status' has the wrong structure
2019-07-24T19:28:13.371937Z 0 [ERROR] Native table 'performance_schema'.'session_status' has the wrong structure
2019-07-24T19:28:13.371968Z 0 [ERROR] Native table 'performance_schema'.'variables_by_thread' has the wrong structure
2019-07-24T19:28:13.371996Z 0 [ERROR] Native table 'performance_schema'.'global_variables' has the wrong structure
2019-07-24T19:28:13.372024Z 0 [ERROR] Native table 'performance_schema'.'session_variables' has the wrong structure
...
```

Sau khi research 1 lúc thì lỗi trên xuất hiện là do crash `information_schema` trong quá trình upgrade, tôi được cân nhắc sử dụng lệnh `mysql_upgrade` để fix.

Nhưng khi chạy xong thì lại xuất hiện thêm 1 lỗi khác nữa. Lỗi này không xuất hiện trong log của MySQL, mà xuất hiện trong log của Cpanel.

![img](/assets/img/cty1-2-15.png)

Lại sau khi research thì lỗi này do bản MySQL 5.7 bắt buộc dùng `explicit_defaults_for_timestamp = 1` trong `my.cnf`.  

![img](/assets/img/cty1-2-16.png)

**Retry** phát nè.

Lỗi nữa luôn, bắt đầu đổ mồ hôi hột rồi nha...

![img](/assets/img/cty1-2-17.png)

Lại sau khi research 1 lúc nữa thì nhận ra rằng lỗi này ... không fix được ![:v](/assets/img/fb-pacman.png).

Thôi thì buồn ngủ quá rồi, check lại 1 vòng các website đang có trên này thì thấy vẫn vào bình thường. Làm liều báo khách phối hợp check lại giúp thôi chứ sao giờ nữa, có lỗi lầm gì thì cài lại nguyên con server như mới cho rồi :-<.

#### 3. Kết luận

4h sáng, đúc kết lại được là Cpanel nhìn document có vẻ dễ dàng, automatic đồ, chạy lèo lèo đồ nhưng khi gặp lỗi 1 phát là debug muốn đổ mồ hôi hột luôn.

Sau này có gặp mấy case liên quan tới Cpanel chắc phải chuẩn bị kỹ càng hơn mới được (mặc dù hôm nay là kỹ càng lắm rồi).

Game này khá khó ![:(](/assets/img/fb-frown.png).

Sleeping ...!
