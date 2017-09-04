# 原因
当我们使用桌面环境的时候通常都会有应用启动器, 主要是为了方便启动应用, 而不用使用命令行启动, 有时都不知道系统预装的软件叫什么名字. 而且如果启动器上有设置好应用, 有些桌面环境还可以搜索, 这样启动应用就变得很方便了.

今天我下载了Eclipse, 直接解压就可以用了, 但是我想设置一个启动图标

# 设置
首先你要想知道你装好的应用的路径, 例如我的Eclipse是装在/home/kris/application/eclipse/eclipse, 然后自带有一个icon的图标, 复制一个icon改格式为PNG

1. 在/usr/share/applications/下创建一个eclipse.desktop的文件
2. 也可以在~/.local/share/applications/下创建,
