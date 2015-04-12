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
	
参考网址:
	1.[git基础教程]
	
	[git]:	http://rogerdudler.github.io/git-guide/index.zh.html
