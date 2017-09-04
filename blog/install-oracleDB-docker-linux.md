title: 使用Docker在Linux下部署Oracle Database
date: 2017/6/1 00:27
comments: true
categories:
  - DataBase
tags:
  - Oracle
  - Linux
  - DataBase
  - Docker
---
Linux上通过Docker运行Oracle Database

由于Oracle DataBase在Linux上很难安装, 所以不想折腾的我使用Docker来部署Oracle Database在Linux上

# 安装前的准备
一开始我了解到可以通过使用Docker来运行Oracle数据库, 马上就安装Docker, 然后运行一个Oracle数据库的镜像, 但第二次开机发现进不去图形界面, 提示
> X session unable to write to the /tmp
原因就是Docker的容器运行过程中会产生很多临时文件, 而我的var目录其实跟/目录是同一个分区, 那么Docker产生的临时文件直接导致我的/分区没有空间, 然后X session没有办法创建临时文件, 所以在安装Docker前, 建议先挂载一个分区到/var/lib/docker.

方便起见, 我使用Gparted将原本的swap分区格式化为ext4格式, 然后挂载到/var/lib/docker目录下, 这样就算Docker占用多少空间都不会导致系统崩溃. 

但是这样做还有要注意的是, 分区完之后可以使用blkid查看分区情况和/etc/fstab的分区表是不是一致, 尤其注意UUID是不是一致, 我之前分区之后导致/etc/fstab中还记录着以前swap分区的信息, 这样导致我每次开机都去检查硬盘, 要等待1min 30s
> a start job running dev-disk-by....
如果出现这种情况, 可以尝试将blkid查询到的不同分区UUID覆盖/etc/fstab中的对应分区的UUID

# 安装Docker
[参照Docker入门到实践](https://www.gitbook.com/book/yeasy/docker_practice)
查看自己的系统
```shell
$ uname -a
Linux Debian 4.9.0-2-amd64 #1 SMP Debian 4.9.18-1 (2017-03-30) x86_64 GNU/Linux
```
我使用的是Debian 9, 通过下面方式安装Docker
```shell
$ curl -sSL https://get.docker.com/ | sh
```
执行这个脚本的之后有一段提示信息
> If you would like to use Docker as a non-root user, you should now consider
> adding your user to the "docker" group with something like:
>
>   sudo usermod -aG docker kris
>
> Remember that you will have to log out and back in for this to take effect!
>
> WARNING: Adding a user to the "docker" group will grant the ability to run
> containers which can be used to obtain root privileges on the
> docker host.
> Refer to https://docs.docker.com/engine/security/security/#docker-daemon-attack-surface
> for more information.
将自己的用户添加到docker组中
```shell
$ sudo usermode -aG docker kris
```
# 启动Docker
Ubuntu 16.04、Debian 8 Jessie/Stretch
```shell
$ sudo systemctl enable docker
$ sudo systemctl start docker
```

# 安装Oracle-11g-XE Docker
因为我只是使用Oracle数据库进行学习开发, 所以只下载Oracle-11g-XE版就好了, 在Docker store上面有12c的版本, 不过体积较大
[Docker Store oracle-11g-XE](https://store.docker.com/community/images/wnameless/oracle-xe-11g)
pull Docker image, Ubuntu 16.04下
```shell
$ sudo docker pull wnameless/oracle-xe-11g
```

# 启动Docker Oracle-11g-XE
因为我需要用宿主机连接数据库, 因此我要开启远程连接
```shell
$ sudo docker run -d -p 49160:22 -p 49161:1521 -e ORACLE_ALLOW_REMOTE=true wnameless/oracle-xe-11g
```

# 连接数据库
我是用Dbeaver连接数据库的, 按以下配置
> hostname: localhost
> port: 49161
> sid: xe
> username: system
> password: oracle
> Password for SYS & SYSTEM: oracle

# 注意事件
一开始我不太会用Docker, 所以每次我都是执行
```shell
$ sudo docker run -d -p 49160:22 -p 49161:1521 -e ORACLE_ALLOW_REMOTE=true wnameless/oracle-xe-11g
```
但这样其实相当于每次都使用那个镜像去开启一个新的容器, 这样我每次插进去数据库的数据也没有了(其实不是没有, 而是每次操作的数据库都不是同一个了)

所以只要运行一次就好了, oracle-XE只能创建一个数据库实例, 那如果用Docker的话, 你开多个容器应该也可以解决只能有一个实例的问题

然后可以通过
```shell
# 列出开启中的容器的信息, 这样我们可以查看刚开启的容器的CONTAINER ID
# 容器的ID跟Git的版本号差不多, 可以只输入前几位就可以识别
$ sudo docker container ls
# 停止/开启container, 假如我的CONTAINER ID是65s4b2c5c2, 那么我可以只输入65s
$ sudo docker container stop 65s
$ sudo docker container start 65s
```
