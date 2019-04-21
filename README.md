## 前言

笔记使用GitBook + Typora + Git（目录管理+编辑+托管）完成。

Gitbook安装和使用指南：https://einverne.github.io/gitbook-tutorial/content/basic.html

SUMMARY.md基本上是列表加目录链接的语法。编辑时加*和空格定义目录级别，最多三级目录，然后编辑目录名和文件名。

1，编辑完目录，命令行gitbook init使生效。

2，编辑完book后使用命令行gitbook serve进行本地预览。执行命令后会对 Markdown 格式的文档进行转换，默认转换为 html 格式，最后提示 “Serving book on [http://localhost:4000](http://localhost:4000/)”。可以打开浏览器查看。

3，整本书大致编辑完毕，你可以执行 gitbook build 命令构建书籍，默认将生成的静态网站输出到 _book 目录。实际上，这一步也包含在 gitbook serve 里面，因为它们是 HTML，所以 GitBook 通过 Node.js 给你提供服务了。 build 命令可以指定路径：

```shell
gitbook build [书籍路径] [输出路径]   #指定路径
gitbook serve --port 2333   #指定端口
gitbook pdf ./ ./mybook.pdf  #生成PDF，路径可省
```

4，最后，使用Git进行书籍的版本生成和发布，在 mybook 目录下执行 git init 初始化仓库，执行 git remote add 添加远程仓库（你得先在远端建好）。接着就可以愉快地 commit，push，pull！