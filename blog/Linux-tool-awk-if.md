最近使用svn的时候, 也行是我还不熟悉svn, 反正我没找到像 `git add .` 的命令
不过 `svn st` 的输出中, 如果是新增的文件或文件夹前面是会有个 ?
那么如果我像一次将所有新增的文件添加到版本库中就可以用下面命令了
```shell
$ svn st | awk '{if($1 == "?") print $2} | xargs svn add'
```