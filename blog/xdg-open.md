title: Linux终端打开默认文件管理器
date: 2017/6/1 23:59
comments: true
categories:
  - Shell Command Line
tags:
  - Linux
  - Shell Command
  - xdg-open
---
有时在Linux终端下面操作很方便, 但是如果想要打开一些文件又很麻烦, 例如图片或者文档. 其实自己根本记不住那些图片浏览软件或者Lirbre Office怎么用命令打开. 所以很多时候都是先打开GUI文件管理器, 然后再去到想要打开文件的路径, 再打开文件. 一直想, 文件管理器有'在终端打开的功能', 要是能够在终端打开文件夹那就方便多了.

Google了一下, 发现有xdg-open这个命令, 使用方法很简单

```shell
# 打开当前目录
$ xdg-open .
# 打开某个路径
$ xdg-open ~
# 打开某个文件
$ xdg-open a.png
```

为了让自己更懒, 再给这个命令取个别名. 在.bashrc中加入
> alias xdg-open="xp"
```shell
$ source .bashrc
$ xp .
```

Enjoy It! :)
