# 在 Terminal 和 GNOME 文件管理器中互相打开文件夹
现在装好Linux 都发现文件管理器中右键菜单有 `Open terminal here` 的功能, 但是有时是想要在终端打开系统文件管理器并且定位到当前文件夹

## 从系统文件管理器打开终端
需要安装文件管理器插件 Nautilus
```shell
$ sudo apt install nautilus
$ sudo apt install nautilus-open-terminal
```
安装完之后就可以在文件管理器中点击右键-->`Open terminal here`

## 从终端打开系统文件管理器
安装完Nautilus之后
打开文件管理器并定位到当前目录
```shell
$ nautilus .
```
