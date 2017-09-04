之前一直用的是Debian+Windows 10双系统, 最近终于找齐了在Linux上开发所需要的各种软件, 而且已经差不多半年没有使用Windows啦, 最主要原因是128G的SSD分配给两个系统不太够用. 最终决定删除Windows, 然后调整分区, 让Linux独占所有硬盘空间.

# 格式化Windows相关的分区

先将Windows系统下面一些有用的文件备份, 使用Gparted将所有ntfs格式的分区全部格式化成etx4.



# 调整分区

将Windows的分区格式化后, 其实这个128G的SSD的分区有点凌乱, 很多不连续的空闲分区, 可以使用Gparted工具调整分区. 不过要注意的几点:

- resize, 只能影响相邻的分区, 所以如果我想增大/dev/sda4, 那么sda3或者sda5必须是空闲分区
- resize/remove的时候那些正在使用的不能操作, 你会看见前面有一个锁的图标, 因此建议先刻录一个Ubuntu启动盘, 然后进入Live系统, 自带Gparted, 然后点击deactive后再操作

*在调整分区的过程中我遇到block size error这个错误, 网上教程是说在使用dd命令制作U盘启动前使用sudo dd if=/dev/zero of=/dev/sdc bs=2048 count=32, 但这个错误依然存在, 不过我忽略掉这个错误继续操作*



# 调整Linux LVM

调整完分区之后那些空间还不能真正被我们的挂载点所使用, 就是说虽然我的sda7(假设)的分区已经重新调整大小了, 但是我的sda7下面的/目录/home目录这些也要调整大小才能使用到空闲的分区

使用命令`sudo pvdisplay -v -m`, 查看逻辑分区的空闲容量

> --- Physical volume ---
>   PV Name               /dev/sda8
>   VG Name               fedora
>   PV Size               103.16 GiB / not usable 1.00 MiB
>   Allocatable           yes (but full)
>   PE Size               4.00 MiB
>   Total PE              26408
>   Free PE               0
>   Allocated PE          26408
>   PV UUID               YFRUay-zTex-87nL-9QPe-K6R8-7A0t-baGwnK
>
>   --- Physical Segments ---
>   Physical extent 0 to 1023:
>   Logical volume	/dev/fedora/swap
>   Logical extents	0 to 1023
>   Physical extent 1024 to 6143:
>   Logical volume	/dev/fedora/root
>   Logical extents	0 to 5119
>   Physical extent 6144 to 19456:
>   Logical volume	/dev/fedora/home
>   Logical extents	0 to 13312
>   Physical extent 19457 to 22016:
>   Logical volume	/dev/fedora/root
>   Logical extents	5120 to 7679
>   Physical extent 22017 to 26407:
>   Logical volume	/dev/fedora/home
>   Logical extents	13313 to 17703

Free PE就是你整合分区之后多出的空闲的容量, 当然我这里已经分配完了

假如现在我们有30G, 我想分配10G给根目录, 然后剩下的给/home目录, 可以用下面的命令

```shell
$ sudo lvextend -l 10G /dev/fedora/root
$ sudo lvextend -l 100%FREE /dev/fedora/home
```

逻辑卷还有其他操作, 这里不详细将

调整分区就完毕了, 现在Windows的所有空间都给了Linux了



# 一些手尾

1. 你可能不想在grub引导界面看见Windows的开机启动, 所以你可以修改/boot/grub/grub.cfg文件, 将

   > \### BEGIN
   >
   > 有关Windows的信息删除
   >
   > \### END

   也可以把time out=10设为0, 直接跳过grub

2. 我操作过程中移动了分区和创建了一个新的Linux swap分区, 因此开机会出现一个running job提示, 要等待1min 30s, 可以[参考](http://kristar.oschina.io/2017/05/21/linux/note/a-start-job-running-dev-disk-by/)如何处理.
