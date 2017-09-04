## TinyBlog 教程
1. 从[TinyBlog的Github主页](https://github.com/YangHanqing/tinyblog)fork一份到你的仓库,更改项目名称为`your_name.github.io`,几分钟后Github会自动为你开通[your_name.github.io](yanghanqing.github.io)的个人主页

2. 修改`about me.md`文件作为你的个人介绍,为了更快的加载速度,也可以选择写死在`index.html`中

3. 写好markdown文件后,保存到`./blog`目录下,依次执行下面的语句即可.如果你不熟悉Git如何使用,请参考Github提供的相关教程.

	> git add .
	> git commit -m "Update blog"
	> git push
	
4. 如果修改了CNAME,记得core.js中把`user`改为自己的账户名,否则通过URL自动获取.

5. 分享文章给他人,可以通过在链接后加如下参数 `?title=文章名`

6. 建议新文章发布后,评论栏留空,并点击一下提交评论按钮,这样会用你自己的账户创建issue,以后如果comment有更新,不会打扰到第一个评论的人.

## 许可
MIT
	




