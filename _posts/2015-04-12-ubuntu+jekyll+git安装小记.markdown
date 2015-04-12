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
	
You’ll find this post in your `_posts` directory. Go ahead and edit it and re-build the site to see your changes. You can rebuild the site in many different ways, but the most common way is to run `jekyll serve`, which launches a web server and auto-regenerates your site when a file is updated.

To add new posts, simply add a file in the `_posts` directory that follows the convention `YYYY-MM-DD-name-of-post.ext` and includes the necessary front matter. Take a look at the source for this post to get an idea about how it works.

Jekyll also offers powerful support for code snippets:

{% highlight ruby %}
def print_hi(name)
  puts "Hi, #{name}"
end
print_hi('Tom')
#=> prints 'Hi, Tom' to STDOUT.
{% endhighlight %}

Check out the [Jekyll docs][jekyll] for more info on how to get the most out of Jekyll. File all bugs/feature requests at [Jekyll’s GitHub repo][jekyll-gh]. If you have questions, you can ask them on [Jekyll’s dedicated Help repository][jekyll-help].

[jekyll]:      http://jekyllrb.com
[jekyll-gh]:   https://github.com/jekyll/jekyll
[jekyll-help]: https://github.com/jekyll/jekyll-help
