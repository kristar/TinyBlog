我地有时候需要为Linux Shell命令设置别名，咁样可以好方便咁打出命令。

例如当输入ll的时候就执行ls -l

可以通过设置命令别名来做到这样的效果

在/etc/bashrc或～/.bashrc中添加
```shell
alias ll='ls -l'
```
保存后执行, 重新读取配置文件
```shell
source ～/.bashrc 
```
在命令行下面键入alias就可以列出所有设置的命令别名了
