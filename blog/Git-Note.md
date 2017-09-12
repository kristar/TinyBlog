# Github 开启二步验证后 Push 失败

今天用命令行 Push 发现总是提示验证失败, 突然想起前几天开了二步验证, 上网查询后发现就是因为这个问题
解决办法:
1. `$ git config --global credential.helper store`
2. 在 Github 上点击 settings --> Personal access tokens
3. Generate new token --> 填写 token 的描述 --> 选择 token 的权限范围(如果只是想要提交代码, 只选择 repo 就够了)
4. 之后会生成一个 token 复制好
5. 再次 `push` 但是这次密码填你刚刚复制的 token

*认证信息会保存在 ~/.git-credentials 文件中*

# 修改 git 的默认编辑器
通常提交的时候使用的是 `git commit -m "message"`
但是这种方式使得提交的信息并不是很详细, 如果我们不加 `-m` 选项的话就会打开 git
的默认编辑器, 通常是 `nano`, 但我的是提示 `vi` 命令找不到, 所以我想替换成 `vim`
1. 方法一: 直接修改 `~/.gitconfig` 文件, 在 `[core]` 下面加上 `editor=vim`
2. 方法二: 使用命令 `git config --global core.editor vim` 直接修改
