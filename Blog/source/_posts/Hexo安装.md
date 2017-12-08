---
title: Hexo安装
date: 2017-07-07 10:25:12
categories: "Hexo"
---

## 安装Git ##
我们很经常会要用到Git以及Github，首先来安装Git：

- [下载Git](https://git-scm.com/downloads)

选择合适的版本下载然后安装，我们稍后将会用上。


## 安装Node.js ##
因为Hexo是一款基于Node.js的静态博客框架，所以我们还需要安装好它：

- [下载Node.js](https://nodejs.org/en/download/)

同样选择对应自己电脑的版本下载安装。

## 安装Hexo ##
安装好Git和Node.js之后，轮到我们今天的主角登场了~先打开Git Bash，然后输入下面这条命令来安装Hexo：

``` bash
$ npm install hexo-cli -g
```

来试试安装成功没有，输入以下命令查看Hexo的版本信息：

``` bash
$ hexo -v
```

如果出现类似内容说明安装成功啦！

    hexo-cli: 0.1.9
	os: Windows_NT 6.1.7601 win32 ia32
	http_parser: 2.3
	node: 0.12.2
	v8: 3.28.73
	uv: 1.4.2-node1
	zlib: 1.2.8
	modules: 14
	openssl: 1.0.1m

## Hexo的使用 ##

现在我们可以开始创建博客写文章了！是不是很快~下面会介绍几个Hexo的常用命令，掌握之后就能愉悦地开始写作之旅了XD

### 新建博客 ###

假设我在E盘有个叫blog的文件夹，现在我们用Git Bash进去这个文件夹里，然后将它初始化为我们的博客目录：

``` bash
$ hexo init
```

安装成功的提示信息：

	INFO  Copying data to E:\blog
	INFO  You are almost done! Don't forget to run 'npm install' before you start blogging with Hexo!

注意看到第二条！提示我们在开始使用前要先执行

``` bash
$ npm install
```

这条命令是用来安装依赖包的，具体安装内容可以在package.json文件里找到。安装好了之后会看到一大串的信息，这里就不贴出来了。现在我们可以看到blog目录下的文件结构是这样的：

	.
	├── _config.yml
	├── node_modules
	├── package.json
	├── scaffolds
	├── source
	|   └── _posts
	└── themes

### 新建文章 ###

``` bash
$ hexo new "我的第一篇文章"
```

### 生成界面 ###

``` bash
$ hexo generate 或者 hexo g
```

### 界面预览 ###

``` bash
$ hexo server 或者 hexo s
```

我们会看到这样一条提示：

	INFO  Hexo is running at http://0.0.0.0:4000/. Press Ctrl+C to stop.

打开浏览器访问http://localhost:4000/

### 配置信息 ###

是不是很激动！成功了有木有~不过等等，博客名写着Hexo，可不是我想的那样噢，还有还有，页面底部的版权信息里作者名字也不是我啊喂！来，我们来改掉它们！

找到博客根目录下的配置文件_config.yml，用自己喜欢的文本编辑器编辑它。看到# Site的那一部分，里面的title就是博客的名字，subtitle就是副标题，author对应的那个就是作者名字啦，把自己的大名写上去吧！

还有很多配置项可以修改，这里就不详细讲了，可以查看[Hexo官方文档](https://hexo.io/docs/configuration.html)对照着修改配置。

### 更换主题 ###

在这个看脸的世界，我们的博客怎么可以没有颜值呢，折腾完上面的那些之后，是时候给博客挑上件漂亮的“衣服”了。

Hexo官方网站上展示了多套主题，可以按自己的口味挑选，或者动手能力强的，可以试着自己写一套独一无二的主题噢XD

这里以[Apollo](https://github.com/pinggod/hexo-theme-apollo)这个主题为例，进去github页面之后可以看到很详细的安装和启动方法，这里就不展开来讲了。

### 添加分类 ###

``` bash
$ hexo new page categories
```

编辑站点的source/categories/index.md，添加

	title: categories
	date: 2015-10-20 06:49:50
	type: "categories"
	comments: false


### 添加标签 ###

```bash
$ hexo new page tags
```

编辑站点的source/tags/index.md，添加

	title: tags
	date: 2015-10-20 06:49:50
	type: "tags"
	comments: false