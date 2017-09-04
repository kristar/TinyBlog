title: Linux安装思源宋体
date: 2017/5/20 12:37
update: 2017/5/20 12:37
comments: true
categories:
  - Linux
tags:
  - Linux
  - 思源宋体
  - source-han-serif
---
# 下载Source-han-serif
思源宋体是Google和Adobe联合制作的, 但命名不是统一, Adobe称为Source-han-serif, 而Google归类为Noto系列

思源宋体是开源的, 你可以在Github上下载[adobe-fonts/source-han-serif](https://github.com/adobe-fonts/source-han-serif/tree/release/SubsetOTF)
你可以在CN目录下下载不同的字重, 也可以一次性下载SourceHanSerifCN.zip包含7种字重
然后解压(例如解压到download目录)

# 以管理员权限安装字体
先复制字体到/usr/share/fonts/SourceHanSerifCN/目录下

```shell
$ sudo mkdir /usr/share/fonts/SourceHanSerifCN
$ sudo cp ~/download/SourceHanSerifCN/* /usr/share/fonts/SourceHanSerifCN/
$ cd /usr/share/fonts/SourceHanSerifCN/
```
安装字体

```shell
$ sudo mkfontscale
$ sudo mkfontdir
$ sudo fc-cache
```

安装成功, 打开LibreOffice会发现字体多了思源宋体或Souce Han Serif字体
