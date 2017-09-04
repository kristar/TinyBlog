参考[Debian Wiki](https://wiki.debian.org/VirtualBox#Debian_9_.22Stretch.22)

# 1. 添加Oracle Virtual的源到/etc/apt/sources.list.d

```shell
# 可以在sources.list.d的目录下面新建一个virtualbox.list
$ sudo vim virtualbox.list
# 添加下面内容到virtualbox.list中, stretch换成你的发行版的名字
deb http://download.virtualbox.org/virtualbox/debian stretch contrib

# 或者直接在/etc/sources.list中添加
```

# 2. 添加Oracle VirtualBox 公钥

```shell
curl -O https://www.virtualbox.org/download/oracle_vbox_2016.asc
sudo apt-key add oracle_vbox_2016.asc
```

# 3. 安装VirtualBox

```shell
sudo apt-get update
sudo apt-get install virtualbox-5.1
```

# 4. 一些可能出现的问题

- 安装过程中可能会提示你缺少一些包, 安装就好
- 还会提示你要求你使用root用户运行/sbin/vboxconfig
- 安装完成


