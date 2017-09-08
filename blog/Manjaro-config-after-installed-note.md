# Manjaro 安装后配置笔记

**你可以先安装 `vim` 再进行下面步骤, 皆因我不太会用 `nano`**

```shell
$ sudo pacman -S vim
```

## 更换源

```shell
$ sudo vim /etc/pacman-mirrors.conf
```

加入或编辑, 保存后退出

> OnlyCountry=China

更新列表

```shell
$ sudo pacman-mirrors -g
```

编辑 /etc/pacman.conf 并加入

> [archlinuxcn]
> SigLevel = Optional TrustedOnly
> Server = https://mirrors.ustc.edu.cn/archlinuxcn/$arch

再执行

```shell
$ sudo pacman -S archlinuxcn-keyring
$ sudo pacman -Syyu		# 更新系统
$ sudo pacman -S google-chrome	# 安装 Google Chrome, 可选	
```

## 安装输入法

```shell
$ sudo pacman -S fcitx-sogoupinyin
$ sudo pacman -S fcitx-im # 全部安装
$ sudo pacmna -S fcitx-configtool # 图形化配置工具
```

打开 `~/.xprofile` 并添加

> export GTK_IM_MODULE=fcitx
> export QT_IM_MODULE=fcitx
> export XMODIFIERS="@im=fcitx"

注销后重新登录并运行 `fcitx-configtool` 添加 sogoupinyin

## Emacs 无法输入中文

表现为系统语言设置为英文, 打开了 Emacs 的 GUI 版
*你可以查看 `fcitx-diagnose` 的错误信息*

编辑 /etc/locale.gen, 取消 zh_CN.UTF-8 UTF-8 一行的注释

> ...
> #zh_CN gbk
> zh_CN.UTF-8 UTF-8
> #zh_TW.UTF-8 UTF-8
> ...

保存退出后执行下面命令生成 locale

```shell
$ sudo locale-gen
```

可以通过编辑 `/etc/locale.conf` 或通过 `localectl` 命令来设置

```shell
$ sudo localectl set-locale LC_CTYPE=zh_CN.UTF-8
```

重新登录后生效或者执行下面命令立即生效(仅对一个会话生效)

```shell
$ LANG= source /etc/profile.d/locale.sh
```

现在 Emacs 可以输入中文了