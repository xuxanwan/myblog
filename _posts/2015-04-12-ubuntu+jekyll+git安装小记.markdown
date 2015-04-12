---
layout: post
title:  "Ubuntu jekyll git使用小记"
date:   2015-04-12 05:42:11
categories: jekyll
---

第一次使用markdown写博文。就用`Jekyll`安装过程，做第一篇博文。
一,安装`Jekyll`
	a,前期准备：装好的ubuntu系统。
	b,`Jekyll`需要ruby环境支持,故先安装ruby环境.{% highlight bash %}sudo apt-get install ruby1.9.1-dev{% endhighlight %}
	c,接着安装`Jekyll`通过如下命令.{% highlight bash %}gem install jekyll # if this fails then sudo gem install Jekyll{% endhighlight %}
	d,若发现报错如下:![Jekyll安装报错.png](/public/upload/Jekyll安装报错.png)则通过如下命令安装js运行环境.{% highlight bash %}apt-get install nodejs{% endhighlight %}
	f,敲入jekyll -v 若显示版本信息则安装成功.
	g,通过 Jekyll new myblog 新建本地博客
	h,通过 jekyll serve启动博客本地预览服务器.这时候可以通过访问127.0.0.1:4000 访问你的博客.
二,托管到github
	a,若没有git环境则安装git   apt-get install git
	b,注册github帐号,并在github上创建新项目,命名为myblog
	c,进入jekyll创建的博客文件夹,使用git init 初始化文件夹.
	d,关联远程我们创建的git仓库:git remote add origin <server>
	e,生成页面分支gh-pages :Git checkout –b gh-pages
	f,将分支推送到远端仓库，要不然远端仓库是没有该分支的:Git push origin gh-pages
	g,添加jekyll生成的文件:Git add ./
	h,提交到缓存区:Git commit –m  ‘description’
	i,提交更改到远程仓库:Git push origin gh-pages




参考网址:
1.[git基础教程]
	
[git]:	http://rogerdudler.github.io/git-guide/index.zh.html
[git]:	http://jekyllrb.com/docs/templates/
[git]:	http://lugir.com/git-basic.html
[git]:	http://www.tuicool.com/articles/Yr6RjuJ
[git]:	http://www.ruanyifeng.com/blog/2012/08/blogging_with_jekyll.html
[git]:	http://blog.csdn.net/on_1y/article/details/19259435