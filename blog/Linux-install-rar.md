# 安装rarlinux

在Debian或者Ubuntu下面，你可以通过下面命令来安装unrar
```shell
# apt-get install unrar
```
或者
```shell
$ sudo apt-get install unrar
```
你也可以在rarlab的网站上下载，然后解压。
```shell
$ tar -zxvf rarlinux-*.tar.gz
$ sudo cp rar unrar /usr/local/bin
```
使用unrar
```shell
# 解压(extract)文件到当前目录
unrar e file.rar
# 列出(List)压缩包里面的内容
unrar l file.rar
# 解压文件
unrar x file.rar
```
