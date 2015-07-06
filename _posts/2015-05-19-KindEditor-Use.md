---
layout: post
title:  "KindEditor使用"
date:   2015-05-19
categories: 前端

---
最近在项目里使用到了富文本编辑框.以前就接触过KindEditor,这次系统的使用,故做些整理.

项目使用的是异步加载替换div的方式实现页面的变换,[kindEditor的初始化](http://kindeditor.net/docs/usage.html)中的` KindEditor.ready() `中使用的方式并不起作用,原因可能是这种情况下DOM加载完成事件(DOMContentLoaded)没被触发,导致新加载的div中的js函数并没有被执行。

## 具体步骤如下:

- 在加载替换div后触发contentChged事件.`$(contentdiv).trigger('contentChged');`
- 捕获contentChged初始化编辑器
{% highlight javascript %}
	$("#push-config-content").bind("contentChged", function () {
		initKindEditor();
	});
{% endhighlight %}
- 具体初始化代码
{% highlight javascript %}

	var editor4Content;
	function initKindEditor(){
		editor4Content = KindEditor.create('textarea[name="pushMedia.pmContent"]', {
			resizeType : 1,
			allowPreviewEmoticons : false,
			allowImageUpload : false,
			afterBlur: function () { this.sync(); },
			basePath: '../js/jquery/plugins/kindeditor-4.1.10/',
			items : [
				'fontname', 'fontsize', '|', 'forecolor', 'hilitecolor', 'bold', 'italic', 'underline',
				'removeformat', '|', 'justifyleft', 'justifycenter', 'justifyright', 'insertorderedlist',
				'insertunorderedlist', '|',  'source'],
			afterChange : function() {
				 //字数统计包含HTML代码,该功能是通过修改源码实现的,更新版本时请注意
			     $('.word_count1').html(this.countCode()); 
			     var limitNum = 512;  //设定限制字数
			     var pattern = '还可以输入' + limitNum + '字'; 
			     $('.word_surplus').html(pattern); //输入显示
			     if(this.countCode() > limitNum) {
			      pattern = ('字数超过限制，请适当删除部分内容');
			      } else {
			      //计算剩余字数
			      var result = limitNum - this.countCode(); 
			      pattern = '还可以输入' +  result + '字'; 
			      }
			      $('.word_surplus').html(pattern); //输入显示
			} 
		}); 
	}
{% endhighlight %}
- 项目需要字数限制和统计,并且字数限制和统计涉及到中文字符串的处理.
{% highlight javascript %}
		/**
		 * 计算字符串所占的内存字节数，默认使用UTF-8的编码方式计算，也可制定为UTF-16
		 * UTF-8 是一种可变长度的 Unicode 编码格式，使用一至四个字节为每个字符编码
		 * 
		 * 000000 - 00007F(128个代码)      0zzzzzzz(00-7F)                             一个字节
		 * 000080 - 0007FF(1920个代码)     110yyyyy(C0-DF) 10zzzzzz(80-BF)             两个字节
		 * 000800 - 00D7FF 
		   00E000 - 00FFFF(61440个代码)    1110xxxx(E0-EF) 10yyyyyy 10zzzzzz           三个字节
		 * 010000 - 10FFFF(1048576个代码)  11110www(F0-F7) 10xxxxxx 10yyyyyy 10zzzzzz  四个字节
		 * 
		 * 注: Unicode在范围 D800-DFFF 中不存在任何字符
		 * {@link http://zh.wikipedia.org/wiki/UTF-8}
		 * 
		 * UTF-16 大部分使用两个字节编码，编码超出 65535 的使用四个字节
		 * 000000 - 00FFFF  两个字节
		 * 010000 - 10FFFF  四个字节
		 * 
		 * {@link http://zh.wikipedia.org/wiki/UTF-16}
		 * @param  {String} str 
		 * @param  {String} charset utf-8, utf-16
		 * @return {Number}
		 */
{% endhighlight %}
故我们需要对中文进行特殊判断.
我的处理方法是,更改KindEditor的源码,在其原生count方法后中增加了:

{% highlight javascript %}

    countCode : function() {
		var self = this,
		total = 0,
		    i,
		    str = _removeBookmarkTag(_removeTempTag(self.html())),
		    len;
		for(i = 0, len = str.length; i < len; i++){
		        charCode = str.charCodeAt(i);
		        if(charCode <= 0x007f) {
		            total += 1;
		        }else if(charCode <= 0x07ff){
		            total += 2;
		        }else if(charCode <= 0xffff){
		            total += 3;
		        }else{
		            total += 4;
		        }
		    }
		return total;
	},
{% endhighlight %}

- 最后贴上html代码

```html
	<div id="content-ke">
		<textarea rows="3" id="push-config-content"class="textarea validate[required,maxSize[512]]" 
			name="pushMedia.pmContent" 
			style="width: 300px;" >${(pushMedia.pmContent)!}</textarea>
		<p> 
		您当前输入了 <span class="word_count1">0</span> 个文字。（字数统计包含HTML代码。）
		<br>
		<span class="word_surplus"></span> 
		</p>
	</div>
```

##使用中碰到的问题
1.在系统中,kindeditor是在tab页下加载的,测试提出的bug是,当tab页面第一次关闭,再次打开的时候,会出现如下报错`VM65105:32 Uncaught TypeError: Cannot read property 'getSelection' of undefined`
经排查,发现是因为关闭后,页面对象被销毁了,而kindeditor对象并没有被销毁的缘故.故在此打开时我的判断kindeditor对象是否存在的函数失效,并未进行重新加载编辑器.
解决方案: 将editor更改为局部变量,值在函数内部生效.当跳出函数时就为null,这样的话就能够再次生成了.
##参考
1. [KindEditor编辑器使用方法](http://kindeditor.net/docs/usage.html)
2. [ KindEditor 4 输入框限定字数](http://blog.csdn.net/myweishanli/article/details/25800185)
