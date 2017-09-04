# 概念

## 非对称加密

![asymmetric-encryption](/img/asymmetric-encryption.svg)

**对称式加解密：**就是加密跟解密都是用同一个密码，别人发给你一个加密的文件和密码，这样你可以通过这个密码来解密这个文件．

**非对称加密：**密码分公钥和私钥，必须用公钥和私钥一起才能解密．当你发送文件的时候使用别人的公钥加密，别人使用他自己的私钥解密．

##　数字签名

为了确保文件是你本人发出的，你可以加上你的数字签名，这样别人就可以确认这是你发出的．



# 安装GnuPG

```shell
$ sudo apt install gnupg
```



# 管理密钥

## 生成密钥

```shell
$ gpg --gen-key
```

这会要求你输入 UID 和邮箱, 例如我输入 frank frank@gmail.com. 这个构成这个密钥的 UID. 生成期间需要产生一些随机位, 你可以不断移动鼠标或者按下键盘加速产生随机位.

## 查看密钥

### 查看公钥

```shell
$ gpg -k
$ gpg --list-keys
```

输出如下

pub 代表公钥, rsa  代表使用 rsa 加密算法, 第二行是 fingerprint 指纹, 用于识别这个公钥

> pub   rsa2048 2017-07-03 \[SC] [expires: 2019-07-03]
>
> ​          7FB47CA743CA31C0681E1C10CEC376A81B4FA0A5
>
> uid           [ultimate] frank \<frank@gmail.com>
> sub   rsa2048 2017-07-03 \[E] [expires: 2019-07-03]

### 查看私钥

```shell
$ gpg -K
$ gpg --list-secret-keys
```

## 导出公钥

```shell
$ gpg --export -a frank > frank.gpg # 加上 -a 会输出 ACSII 码形式的公钥, 容易查看, 复制
```

## 导入公钥

```shell
$ gpg --import frank.gpg
```

## 删除密钥

要删除公钥, 必须先删除私钥

```shell
$ gpg --delete-secret-keys frank
$ gpg --delete-keys frank
```

## 修改密钥

```shell
$ gpg --edit-key frank
```

然后进入交互界面进行修改, 输入help查看帮助



# 加密解密

## 加密文件

假如我们要发送一个 file 文件给 frank, 那么我们就要用 frank 的公钥来加密文件

```shell
$ gpg -er frank file  # -e 代表 --encrypt 加密, -r 代表 Recipient 接收者
```

为了让 frank 知道这个是我发送的, 通常还要加上数字签名, 看下文数字签名

## 解密文件

```shell
$ gpg -d file.gpg > file  # -d 代表 --decrypt 解密, 并输出到 file 文件
```



# 数字签名

如果我们想别人确认这是我们发布的文件, 我们应该在网站发布一串数字签名, 然后对文件进行签名, 那么别人就可以验证这个文件, 然后对比网站上面的数字签名就可以知道下载的文件是不是合法的.

## 签名并压缩

```shell
$ gpg -s file 				# 会生成一个 file.gpg
$ gpg -u frank -s file 		# 可以指定用哪个 UID 来签名
$ gpg -d file.gpg > file	# 解压文件
```

## 签名但不压缩

```shell
$ gpg --clearsign file
```

## 独立出签名文件

上面两种方法都是将签名加入到源文件中, 如果是二进制文件, 我们可不想这么做, 我们需要将签名文件独立成一个文件

```shell
$ gpg -b file # -b 代表 --detach-sign
```

这会产生一个 file.sig 文件, 验证 file.sig 文件的时候要求 file 文件必须存在, 且名字不能改变.

## 验证签名

```shell
$ gpg --verify file.gpg
$ gpg --verify file.asc
$ gpg --verify file.sig  # 要求 file 文件必须存在
```

## 签名且加密

```shell
$ gpg [-u Sender] [-r Recipient] [-a] -es file 
```

# 参考
[gnupg.org](https://gnupg.org/howtos/zh/index.html)

