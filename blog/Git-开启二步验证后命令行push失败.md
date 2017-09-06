# Github 开启二步验证后 Push 失败

今天用命令行 Push 发现总是提示验证失败, 突然想起前几天开了二步验证, 上网查询后发现就是因为这个问题
解决办法:
1. `$ git config --global credential.helper store`
2. 在 Github 上点击 settings --> Personal access tokens
3. Generate new token --> 填写 token 的描述 --> 选择 token 的权限范围(如果只是想要提交代码, 只选择 repo 就够了)
4. 之后会生成一个 token 复制好
5. 再次 `push` 但是这次密码填你刚刚复制的 token

*认证信息会保存在 ~/.git-credentials 文件中*
