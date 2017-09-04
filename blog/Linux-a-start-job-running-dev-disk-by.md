最近开机的时候总是会出现一句话

> s start job running dev-disk-by.....

之前我重新分过区，将原本的4Gswap分区划分为一个3G挂载给docker用，然后1G空着

然后我在[Ask Ubuntu](https://askubuntu.com/questions/711016/slow-boot-a-start-job-is-running-for-dev-disk-by)看见一个帖子也是出现这种问题， 然后是说有一些分区没有用到也没有挂载，所以会出现上面的情况。例如我的那1G空间。

然后我就把那1G划分为swap分区， 重启后还是不行

使用blkid查看分区表，和查看/etc/fstab的分区表发现swap分区的UUID是不一样的

将blkid查到的UUID复制到/etc/fstab的中， 重启就可以了

[解决a start job is running for dev-disk-by启动错误](https://www.linuxdashen.com/%E8%A7%A3%E5%86%B3a-start-job-running-dev-disk-by%E5%90%AF%E5%8A%A8%E9%94%99%E8%AF%AF)
