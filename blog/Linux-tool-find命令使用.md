在Linux下面找一些文件或者批量修改一些文件使用find命令很方便。

语法：find [path…] [expression]

常用格式:
```shell
# 在path路径下找到所有html文件并删除
# -exec 最后必须以\;结束
# {} 代表的是找到的文件的路径名(相对路径)
find path -name "*.html" -exec rm {} \;

# 将path下面的目录结构复制到./tmp目录下
find path -type d -exec cp {} tmp/{} \;
```
如果没有-exec, 默认是-print, 打印找到的文件或路径名

-type的参数有

- b 特殊块文件(缓冲的)
- c 特殊字符文件(不缓冲)
- d 目录
- p 命名管道 (FIFO)
- f 普通文件
- l 符号链接
- s 套接字
- D 门 (Solaris 特有)
