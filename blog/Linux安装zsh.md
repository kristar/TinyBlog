title: Linux安装zsh
date: 2017/06/15 10:35
comments: true
categories:
  - Linux
tags:
  - Shell
  - oh-my-zsh
---
# 安装zsh

```shell
$ sudo apt-get install zsh
```



# 安装oh-my-zsh

[参考github.com/robbyrussell/oh-my-zsh](https://github.com/robbyrussell/oh-my-zsh)

**基本安装:**

**通过curl下载:**

```shell
sh -c "$(curl -fsSL https://raw.githubusercontent.com/robbyrussell/oh-my-zsh/master/tools/install.sh)"
```

**通过wget下载**

```shell
sh -c "$(wget https://raw.githubusercontent.com/robbyrussell/oh-my-zsh/master/tools/install.sh -O -)"
```



# 修改默认shell

```shell
chsh -s /bin/zsh
```

需要注销, 模拟终端的才有效



# 安装主题

到[主题列表](https://github.com/robbyrussell/oh-my-zsh/wiki/External-themes)中下载并安装主题



# 一些错误解决

- **安装完主题后主题乱码:** 你需要按照powerline字体例如 [powerline/fonts](https://github.com/powerline/fonts), 然后更改模拟终端的配置文件的字体, 使用标有powerline的字体