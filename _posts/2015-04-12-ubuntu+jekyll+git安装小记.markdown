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
	
You��ll find this post in your `_posts` directory. Go ahead and edit it and re-build the site to see your changes. You can rebuild the site in many different ways, but the most common way is to run `jekyll serve`, which launches a web server and auto-regenerates your site when a file is updated.

To add new posts, simply add a file in the `_posts` directory that follows the convention `YYYY-MM-DD-name-of-post.ext` and includes the necessary front matter. Take a look at the source for this post to get an idea about how it works.

Jekyll also offers powerful support for code snippets:

{% highlight ruby %}
def print_hi(name)
  puts "Hi, #{name}"
end
print_hi('Tom')
#=> prints 'Hi, Tom' to STDOUT.
{% endhighlight %}

Check out the [Jekyll docs][jekyll] for more info on how to get the most out of Jekyll. File all bugs/feature requests at [Jekyll��s GitHub repo][jekyll-gh]. If you have questions, you can ask them on [Jekyll��s dedicated Help repository][jekyll-help].

[jekyll]:      http://jekyllrb.com
[jekyll-gh]:   https://github.com/jekyll/jekyll
[jekyll-help]: https://github.com/jekyll/jekyll-help
