---
title: mysql配置允许远程连接
date: 2018-03-26 14:39:07
tags: mysql
---

在服务器安装mysql默认情况下，mysql只允许本地登录。
> 服务器系统(Ubuntu)
> sudo apt-get install mysql-server

### 连接远程服务器
> mysql -h服务器ip -u用户名 -p

### 如果出现拒绝连接的信息，在服务器修改mysql的一下配置

1.修改配置文件
  >  /etc/mysql/mysql.conf.d/mysqld.cnf		//或者/etc/mysql/my.cnf 
  > 修改bind_address = 0.0.0.0 

2.修改mysql库的user表的host字段的值，从localhost修改为%
``` sql
//在服务器登陆mysql.
mysql>use mysql;
mysql>update user set host = '%' where user = 'root';
mysql>select host, user from user;
```
修改为这样：
![修改为如图这样](../../image/mysql.jpg)

3.重启mysql服务
> service mysql restart

4.测试连接，成功。

#END
