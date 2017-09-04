title: Linux下使用iconv批量转换GBK文件为UTF-8编码
date: 2017/5/16 21:27
comments: true
categories:
  - Shell Command Line
tags:
  - find
  - iconv
  - Linux
  - Shell Command
---
# 起因

最近想使用brackets(一个web开发IDE), 但是提示仅支持UTF-8编码, 但是有一个项目是从windows复制到Linux下的, 所以都是GBK编码, 于是在网上找到下面方法批量转换文件的字符集编码

# 查看文件编码
```shell
$ file -i filename # 检查filename这个文件的字符集编码
```
# 递归转换
```shell
# 将sourceDir目录下的目录结构复制到utf目录下
find sourceDir -type d -exec mkdir -p utf/{} \;
# 将sourceDir目录下所有文件从GBK编码转换成UTF-8编码
find sourceDir -type f -exec iconv -f GBK -t UTF-8 {} -o utf/{} \;
```
注意: 如果原来的文件就是UTF-8编码, 那么转换之后就会出现乱码, 所以要先查看源文件是不是含有utf-8编码, 如果源文件夹中含有二进制文件例如图片, 那么也会报废

```shell
# 递归查看sourceDir目录下所有文件的字符编码并重定向到/tmp/a文件中
$ find sourceDir -type f -exec file -i {} \; > /tmp/a
# 查看是否含有utf-8编码
$ grep "charset=utf-8" /tmp/a
# 你还可以查看每个文件是什么编码
$ cat /tmp/a
```
# iconv的选项
iconv有如下选项可用:

输入/输出格式规范：-f, –from-code=名称

原始文本编码-t, –to-code=名称 输出编码

信息：-l, –list 列举所有已知的字符集

输出控制：-c 从输出中忽略无效的字符-o, –output=FILE 输出文件-s, –silent 关闭警告–verbose 打印进度信息
