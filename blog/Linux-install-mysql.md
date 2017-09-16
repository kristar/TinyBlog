# 前言
公司最近要我把服务器上的 MySQL 的版本升级一下, 原本 MySQL 的版本是5.4的, 应该是从源安装的,然后要升级到最新的稳定版, 写这篇文章时最新的稳定版时5.7.19. 在此记录卸载与安装 MySQL 的过程

# 备份原有数据库
```shell
# mysqld db > /home/db.sql
```

# 卸载 MySQL
原本安装的时候应该是在源安装的, 所以可以使用包管理器卸载 MySQL
```shell
# rpm -qa | grep mysql // 查询 MySQL 相关的包
# yum remove mysql mysql-server mysql-libs compat-mysql51	// 卸载上面查询到的包
```

## 删除 MySQL 服务
```shell
# chkconfig --list | grep -i mysql
# chkconfig --del mysql
```

## 删除 MySQL 的文件
```shell
# rm /etc/my.cnf
# whereis mysql
```
删除上面找到的目录

# 安装 MySQL
在[MySQL 的官网下载地址](https://dev.mysql.com/downloads/mysql/)中下载适合你自己的 MySQL 版本
而我是先找到 MySQl 的下载按钮, 然后右键点击 Cpoy link address.. 然后在浏览器输入看是不是下载链接, 然后在服务器上使用 `wget` 命令下载 MySQL Community Server 的tar.gz压缩包

通过下面命令安装 MySQL
```shell
# groupadd mysql
# useradd -r -g mysql -s /bin/false mysql
# cd /usr/local
# tar zxvf /path/to/mysql-VERSION-OS.tar.gz
# ln -s full-path-to-mysql-VERSION-OS mysql
# cd mysql
# mkdir mysql-files
# chmod 750 mysql-files
# chown -R mysql .
# chgrp -R mysql .
# bin/mysql_install_db --user=mysql    # MySQL 5.7.5
# bin/mysqld --initialize --user=mysql # MySQL 5.7.6 and up
# bin/mysql_ssl_rsa_setup              # MySQL 5.7.6 and up
# chown -R root .
# chown -R mysql data mysql-files # 这一步， 如果提示data not such file or dir的话创建data目录
# bin/mysqld_safe --user=mysql &
// Next command is optional
# cp support-files/mysql.server /etc/init.d/mysql.server
```

在输入 `bin/mysqld --initialize --user=mysql` 的时候同时设置了一个初始的管理员账号root, 密码会显示出来, 先保存好

然后可以先启动 MySQL , 修改管理员密码
```shell
# /etc/init.d/mysql.server start
# mysql -u root -p
mysql>
Enter password: // 输入刚刚的临时密码

```



# 创建用户

创建用户并授权

```mysql
-- 创建一个可以在任意IP登录的用户
CREATE USER root@'%' IDENTIFIED BY 'Password';
-- 授予权限
GRANT all privileges  ON root.* TO 'root'@'%'  identified by 'Password';
-- 刷新权限
flush privileges;

-- 创建数据库
CREATE database database_name;
-- 创建其他用户
CREATE USER test IDENTIFIED BY 'Password';
-- 授权给test用户
GRANT all privileges ON test.* TO test identified by 'Password';
-- 刷新权限
flush privileges;
```



# 错误

> [Err] 1055 - Expression #1 of ORDER BY clause is not in GROUP BY clause and contains nonaggregated column 'information_schema.PROFILING.SEQ' which is not functionally dependent on columns in GROUP BY clause; this is incompatible with sql_mode=only_full_group_by

**解决方法**

/etc/my.cnf 文件最后增加以下两行配置

>  lower_case_table_names=1
>
>  sql_mode='STRICT_TRANS_TABLES,NO_ZERO_IN_DATE,NO_ZERO_DATE,ERROR_FOR_DIVISION_BY_ZERO,NO_AUTO_CREATE_USER,NO_ENGINE_SUBSTITUTION'



## 启动时出错

如果是在Centos下安装的话， 默认就会有一个/etc/my.cnf文件。 里面配置了mariadb的配置， 可以把里面的内容删掉再配置上面的lower_case_table_names=1