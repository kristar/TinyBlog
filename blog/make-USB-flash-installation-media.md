# 在 Linux 下使用 `dd` 命令制作U盘启动
在 Linux 下制作U盘启动很方便
你需要准备好系统镜像

## 格式化U盘
先查询设备
```shell
$ sudo fdisk -l
```
根据设备的容量找到U盘的设备文件名称， 例如/dev/sdc4，先卸载， 然后格式化

```shell
$ sudo umount /dev/sdc*
$ sudo mkfs.vfat /dev/sdc -I
```

## 制作U盘启动

```shell
$ sudo dd if=~/home/frank/Debian.iso of=/dev/sdc
```
等待几分钟， 即完成
