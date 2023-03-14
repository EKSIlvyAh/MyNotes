## 1、我应该如何合并冲突？

冲突合并一般是因为自己的本地做的提交和服务器上的提交有差异，并且这些差异中的文件改动，Git不能自动合并，那么就需要用户手动进行合并

如我这边执行`git pull origin master`

#### 如果Git能够自动合并，那么过程看起来是这样的

![输入图片说明](http://git.oschina.net/uploads/images/2016/0226/113507_cca8cd22_62561.gif "在这里输入图片标题")

拉取的时候，Git自动合并，并产生了一次提交。

#### 如果Git不能够自动合并，那么会提示

![输入图片说明](http://git.oschina.net/uploads/images/2016/0226/113621_dbc985b5_62561.png "在这里输入图片标题")

这个时候我们就可以知道`README.MD`有冲突，需要我们手动解决，修改`README.MD`解决冲突

![输入图片说明](http://git.oschina.net/uploads/images/2016/0226/113823_fffe18cf_62561.png "在这里输入图片标题")

可以看出来，在1+1=几的这行代码上产生了冲突，解决冲突的目标是保留期望存在的代码，这里保留1+1=2，然后保存退出。

![输入图片说明](http://git.oschina.net/uploads/images/2016/0226/114159_426b8d65_62561.png "在这里输入图片标题")

退出之后，确保所有的冲突都得以解决，然后就可以使用`git add .` `git commit -m "fixed conflicts"` `git push origin master`即可完成一次冲突的合并。

整个过程看起来是这样的

![输入图片说明](http://git.oschina.net/uploads/images/2016/0226/114058_429e8b54_62561.gif "在这里输入图片标题")