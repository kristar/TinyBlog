# 升级 Manjaro 17.04 之后发现 Fcitx 无法调出
通过查看 fcitx-diagnose 发现说 fcitx 没有启动， 而且建议添加到 autostart 中
但是我手动通过图形界面打开过 fcitx， 按理说应该启动了。 于是我在终端中启动， 然后报了一个错误

> fcitx-keyboard-cm-mmuock-already-exists

网上的解决方法是删除 `~/.config/fcitx/` 下所有文件再重新登录, 问题就解决了.