### 小结

1. Git分支十分强大，在团队开发中应该充分应用。

2.合并分支时，加上`--no-ff`参数就可以用普通模式合并，合并后的历史有分支，能看出来曾经做过合并，而`fast forward`合并就看不出来曾经做过合并。

###正文
通常，合并分支时，如果可能，Git会用Fast forward模式，但这种模式下，删除分支后，会丢掉分支信息。

如果要强制禁用Fast forward模式，Git就会在merge时生成一个新的commit，这样，从分支历史上就可以看出分支信息。

**首先，仍然创建并切换dev分支：**
**修改readme.txt文件，并提交一个新的commit：**
**然后切换回master：**
![上述三步](https://upload-images.jianshu.io/upload_images/14597179-26fdbcbf3eac114b.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
**合并dev分支，请注意--no-ff参数，表示禁用Fast forward**。因为本次合并要创建一个新的commit，所以加上-m参数，把commit描述写进去
![禁用Fast forward](https://upload-images.jianshu.io/upload_images/14597179-9baca30093697aca.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
**合并后，我们用`git log`看看分支历史：**
![image.png](https://upload-images.jianshu.io/upload_images/14597179-f4280577935fc9f6.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
可以看到，不使用`Fast forward`模式，merge后就像这样：

![git-no-ff-mode](http://upload-images.jianshu.io/upload_images/14597179-4319afa19f9c1601?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
### 分支策略

在实际开发中，我们应该按照几个基本原则进行分支管理：

首先，`master`分支应该是非常稳定的，也就是仅用来发布新版本，平时不能在上面干活；

那在哪干活呢？干活都在`dev`分支上，也就是说，`dev`分支是不稳定的，到某个时候，比如1.0版本发布时，再把`dev`分支合并到`master`上，在`master`分支发布1.0版本；

你和你的小伙伴们每个人都在`dev`分支上干活，每个人都有自己的分支，时不时地往`dev`分支上合并就可以了。

所以，团队合作的分支看起来就像这样：

![git-br-policy](http://upload-images.jianshu.io/upload_images/14597179-301558680fcd8407?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

