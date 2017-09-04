有时候安装完 Linux 系统之后并没有 sudo 命令， 而安装完 sudo 命令之后还需要将用户添加到 sudo 组中才能使用。
```shell
# usermod -a -G YourUserName sudo
```