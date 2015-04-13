---
layout: post
title:  "Ubuntu jekyll git使用小记"
date:   2015-04-12 05:42:11
categories: jekyll
---

第一次使用markdown写博文。就用Jekyll安装过程，做第一篇博文。

### 一,安装Jekyll

* a,前期准备：装好的ubuntu系统。
* b,Jekyll需要ruby环境支持,故先安装ruby环境.```sudo apt-get install ruby1.9.1-dev```
* c,接着安装Jekyll通过如下命令.```
	gem install jekyll # if this fails then sudo gem install Jekyll```
* d,若发现报错如下:![Jekyll安装报错.png](/public/upload/Jekyll安装报错.png)则通过如下命令安装js运行环境.```apt-get install nodejs```
* e,敲入```jekyll -v ```若显示版本信息则安装成功.
* f,通过``` Jekyll new myblog``` 新建本地博客
* g,通过 ```jekyll serve```启动博客本地预览服务器.这时候可以通过访问127.0.0.1:4000 访问你的博客.

### 二,托管到github

* a,若没有git环境则安装```git apt-get install git```
* b,注册github帐号,并在github上创建新项目,命名为myblog
* c,进入jekyll创建的博客文件夹,使用```git init``` 初始化文件夹.
* d,关联远程我们创建的git仓库:```git remote add origin <server>```
* e,生成页面分支gh-pages :```git checkout –b gh-pages```
* f,将分支推送到远端仓库，要不然远端仓库是没有该分支的:```Git push origin gh-pages```
* g,添加jekyll生成的文件:```git add ./```
* h,提交到缓存区:```git commit –m  ‘description’```
* i,提交更改到远程仓库:```git push origin gh-pages```




### 三,参考网

* 1.[git - 简明指南](http://rogerdudler.github.io/git-guide/index.zh.html)
* 2.[Markdown标签库](http://jekyllrb.com/docs/templates/)
* 3.[git-基本用法](http://lugir.com/git-basic.html)
* 4.[Jekyll博客搭建教程](http://www.tuicool.com/articles/Yr6RjuJ)
* 5.[搭建一个免费的，无限流量的Blog----github Pages和Jekyll入门](http://www.ruanyifeng.com/blog/2012/08/blogging_with_jekyll.html)
* 6.[一个简单易用的markdown编辑器](http://jbt.github.io/markdown-editor)
