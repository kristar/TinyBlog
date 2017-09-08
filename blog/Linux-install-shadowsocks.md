# 在 Linux 中安装并配置 Shadowsocks

我前天使用的是 Debian, 保留了 home 目录后安装了 Manjaro 之后再安装 Shadowsocks-qt5 之后 ss 自动获取了我原来的配置, 并且显示连接成功. 但是不知道为何 Chrome 和 Telegram 就是无法连接. 于是在有人建议我先使用 Shadowsocks-libev 试试
后来发现使用 shadowsocks 或者 shadowsocks-libev 会更简单易用. 这里仅讨论如何安装和配置客户端

首先我使用的是 Manjaro

先安装 Shadowsocks 或 Shadowsocks-libev

```shell
$ sudo pacman -S shadowsocks
```

然后 Shadowsock 会生成一个 `/etc/shadowsocks/example.json` 的文件

> {
>	"server":"remote-shadowsocks-server-ip-addr",
>	"server_port":443,
>	"local_address":"127.0.0.1",
>	"local_port":1080,
>	"password":"your-passwd",
>	"timeout":300,
>	"method":"chacha20-ietf",
>	"fast_open":false,
>	"workers":1
> }

填写好配置之后可以通过 `$ ss-local -c /etc/shadowsocks/example.json` 来运行
不过我们可以使用 `systemctl` 命令

```shell
$ sudo systemctl start shadowsocks@example # 启动服务
$ sudo systemctl enable shadowsocks@example # 设置开机自启动
```

[参考 Arch Linux Wiki](https://wiki.archlinux.org/index.php/Shadowsocks)