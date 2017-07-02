##angualr ueditor 使用




1. `通过bower install angular-ueditor --save` 引入js
2. 引入
```
<script type="text/javascript" src="http://ueditor.baidu.com/ueditor/ueditor.config.js"></script>
<script type="text/javascript" src="http://ueditor.baidu.com/ueditor/ueditor.all.js"></script>
```
不引入的话,会报```Please import the local resources of ueditor!```这个错

问题, 通过bower安装ueditor后,我发现并没有上面两个配置文件,所以我把配置文件给download下来,放到了项目里,然后我发现仅仅这两个文件是不够的
3. 在页面中引用
```html
<div class="form-group">
    <label>正文：</label>
    <!--<textarea class="form-control" rows="3" id="content"></textarea>-->
    <div class="ueditor" config="config" ng-model="materialData.content"></div>
</div>
```
4. 设置配置文件
```javascript

/**
 * Ueditor Angular bar config
 * @type {{toolbars: *[]}}
 */
$scope.config = {
    toolbars: [
        [

            'undo', //撤销
            'redo', //重做
            '|',
            'fontsize', //字号
            '|',
            'blockquote', //引用
            'horizontal', //分隔线
            '|',
            'removeformat', //清除格式
            'formatmatch', //格式刷
            'link', //超链接
            'unlink', //取消链接
            '|',
            'bold', //加粗
            'italic', //斜体
            'underline', //下划线
            'forecolor', //字体颜色
            'backcolor', //背景色
            '|',
            'indent', //首行缩进
            'justifyleft', //居左对齐
            'justifyright', //居右对齐
            'justifycenter', //居中对齐
            'justifyjustify', //两端对齐
            '|',
            'rowspacingtop', //段前距
            'rowspacingbottom', //段后距
            'lineheight', //行间距
            '|',
            'insertorderedlist', //有序列表
            'insertunorderedlist', //无序列表
            '|',
            'imagenone', //默认
            'imageleft', //左浮动
            'imageright', //右浮动
            'imagecenter', //居中
        ]
    ]
}
```

###参考

- [angular ueditor github地址](https://github.com/zqjimlove/angular-ueditor)