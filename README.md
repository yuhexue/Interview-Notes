## 前言

### 文档工具使用：

笔记使用GitBook + Typora + Github（目录管理+编辑+托管）完成。

工具使用教程https://blog.csdn.net/lu_embedded/article/details/81100704

Gitbook安装和使用指南：https://einverne.github.io/gitbook-tutorial/content/basic.html

SUMMARY.md基本上是列表加目录链接的语法。编辑时加*和空格定义目录级别，最多三级目录，然后编辑目录名和文件名。

1，编辑完目录，命令行gitbook init使生效。

2，编辑完book后使用命令支持本地预览或生成PDF文件。可生成 html 格式，静态文件生成在_book目录，最后提示 “Serving book on [http://localhost:4000](http://localhost:4000/)”。在浏览器打开上述本地地址查看。或者生成PDF，生成PDF需要安装插件，具体方法如下：https://blog.csdn.net/weixin_43799175/article/details/84503642

```shell
gitbook build [书籍路径] [输出路径]   #指定路径
gitbook serve --port 2333   #指定端口
gitbook pdf ./ ./mybook.pdf  #生成PDF，路径可省
```

3，使用Github进行书籍的版本生成和发布，预先建好仓库，将文件保存到本地仓库，在github客户端进行change和sync。除了可将md文件发布到github，还可以将静态html发布到github，gitbook(被墙了)。

4，typora的编辑一般采用文件树视图，然后在大纲查看文档知识点。图片插入，在图片全局设置里优先使用相对路径并设置相对路径，图片在插入后，会在相对路径复制图片源，以后文档会去相对路径读取图片，另外typora可设置为自动保存。

总结，在typora离线编辑，将更改放置到github的本地仓库相应目录下，change并sync即可更新版本，并在线浏览。使用gitbook进行本地预览。类似的文档编辑器还有vscode和sublime等。