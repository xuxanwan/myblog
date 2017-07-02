## svn分支管理
### svn目录结构 ###
svn下的目录结构一般分为
```
 myproject/
      trunk/
      branches/
      tags/
```
- 主干(trunk)
	- 是用来做主方向开发的，一个新模块的开发，这个时候就放在trunk，当模块开发完成后，需要修改，就用branch。 
- 分支(branch )
	- 是用来做并行开发的，这里的并行是指和trunk进行比较。
- 标记(tag)
	- 是用来做一个里程碑(milestone)的，不管是不是发布版本，但都是一个可用的版本。这里，应该是只读的。更多的是一个显示用的，给人一个可读的标记。

###对于分支的操作###
#### 新建分支####
- `svn copy http://example.com/repos/myproject/trunk http://example.com/repos/myproject/branches/releaseForAug -m "create branch for release on August"`
	- 这条命令是指给myproject这个repo创建一个名为releaseForAug的branch，使用-m来加入描述。
-  `svn checkout http://example.com/repos/myproject/branches/releaseForAug` 
	-  来迁出你的Branch源文件，在上面进行修改和提交了。其实SVN并没有Branch的内部概念。我们只是创建了一个repo的副本，并自己赋予这个副本作为Branch的意义，所以这与git中的Branch有很大不同。**需要注意的是Branch和Trunk使用同一套版本号，也就是说无论在Branch还是Trunk的提交都会引起主版本号的增加**。这是因为svn copy只支持同一个repository内的文件copy，并不支持跨repository的copy，所以新**创建的Branch和Trunk都属于同一个repository**。

#### 删除分支####
- `svn rm http://example.com/repos/myproject/branches/releaseForAug -m "reason"`

#### 合并分支####
场景:假设现在Branch上fix了一系列的bug，现在我们想把针对Branch的改变同步到Trunk上，那么应该怎么做那？
1. 保证当前Branch分支是clean的，也就是说使用svn status看不到任何的本地修改。
2. 命令行下切换到Trunk目录中
	-  `svn merge http://example.com/repos/myproject/branches/releaseForAug `来将Branch分支上的改动merge回Trunk下。
	- `svn merge  http://example.com/repos/myproject/branches/releaseForAug -r150:HEAD`将Branch的从版本150到当前版本的所有改动都合并到Trunk中。
3. 如果出现merge冲突则进行解决，然后执行`svn ci -m 'description'`来提交变动。
4. 使用 svn mergeinfo
	- `svn mergeinfo http://example.com/repos/myproject/branches/releaseForAug`查看当前Branch中已经有那些改动已经被合并到Trunk中
	- `svn merginfo http://example.com/repos/myproject/branches/releaseForAug --show-revs eligible`查看Branch中那些改动还未合并。

#### 注意点 ####
1. 在使用svn copy时碰到错误 `svn: E205009: Local, non-commit operations do not take a log message or revision
 properties` 解决方式: 将-m '' 改为 -m "" 就可以了

###参考###
[http://panfuy.iteye.com/blog/1278865](http://panfuy.iteye.com/blog/1278865)
[http://www.cnblogs.com/huang0925/p/3254243.html](http://www.cnblogs.com/huang0925/p/3254243.html)
[分支删除](http://stackoverflow.com/questions/3816626/deleting-an-svn-branch)
