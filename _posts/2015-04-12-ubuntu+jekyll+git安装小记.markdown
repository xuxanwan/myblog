---
layout: post
title:  "Ubuntu jekyll gitʹ��С��"
date:   2015-04-12 05:42:11
categories: jekyll
---

��һ��ʹ��markdownд���ġ�����`Jekyll`��װ���̣�����һƪ���ġ�
һ,��װ`Jekyll`
	a,ǰ��׼����װ�õ�ubuntuϵͳ��
	b,`Jekyll`��Ҫruby����֧��,���Ȱ�װruby����.{% highlight bash %}sudo apt-get install ruby1.9.1-dev{% endhighlight %}
	c,���Ű�װ`Jekyll`ͨ����������.{% highlight bash %}gem install jekyll # if this fails then sudo gem install Jekyll{% endhighlight %}
	d,�����ֱ�������:![Jekyll��װ����.png](/public/upload/Jekyll��װ����.png)��ͨ���������װjs���л���.{% highlight bash %}apt-get install nodejs{% endhighlight %}
	f,����jekyll -v ����ʾ�汾��Ϣ��װ�ɹ�.
	g,ͨ�� Jekyll new myblog �½����ز���
	h,ͨ�� jekyll serve�������ͱ���Ԥ��������.��ʱ�����ͨ������127.0.0.1:4000 ������Ĳ���.
��,�йܵ�github
	a,��û��git������װgit   apt-get install git
	b,ע��github�ʺ�,����github�ϴ�������Ŀ,����Ϊmyblog
	c,����jekyll�����Ĳ����ļ���,ʹ��git init ��ʼ���ļ���.
	d,����Զ�����Ǵ�����git�ֿ�:git remote add origin <server>
	e,����ҳ���֧gh-pages :Git checkout �Cb gh-pages
	f,����֧���͵�Զ�˲ֿ⣬Ҫ��ȻԶ�˲ֿ���û�и÷�֧��:Git push origin gh-pages
	g,���jekyll���ɵ��ļ�:Git add ./
	h,�ύ��������:Git commit �Cm  ��description��
	i,�ύ���ĵ�Զ�ֿ̲�:Git push origin gh-pages




�ο���ַ:
1.[git�����̳�]
	
[git]:	http://rogerdudler.github.io/git-guide/index.zh.html
[git]:	http://jekyllrb.com/docs/templates/
[git]:	http://lugir.com/git-basic.html
[git]:	http://www.tuicool.com/articles/Yr6RjuJ
[git]:	http://www.ruanyifeng.com/blog/2012/08/blogging_with_jekyll.html
[git]:	http://blog.csdn.net/on_1y/article/details/19259435