#小结
1. 修复bug时，我们会通过创建新的bug分支进行修复，然后合并，最后删除；

2. 当手头工作没有完成时，先把工作现场git stash一下，然后去修复bug，修复后，再git stash pop，回到工作现场。

#正文
**当前正在dev上进行的工作还没有提交,但要先建立一个issue-101分支来修复BUG：**
![保存现场](https://upload-images.jianshu.io/upload_images/14597179-0d67e4e369d30cea.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
**Git提供了一个stash功能，可以把当前工作现场“储藏”起来，等以后恢复现场后继续工作：**
![git stash](https://upload-images.jianshu.io/upload_images/14597179-69249ee4b5c583e6.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
![上次commit后的状态](https://upload-images.jianshu.io/upload_images/14597179-af8c250796d0150e.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
![git status查看工作区，就是干净的（除非有没有被Git管理的文件），因此可以放心地创建分支来修复bug](https://upload-images.jianshu.io/upload_images/14597179-5f2306c620ffeb4e.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
首先确定要在哪个分支上修复bug，假定需要在master分支上修复，就从master创建临时分支：

![创建Bug分支](https://upload-images.jianshu.io/upload_images/14597179-8c5231f3b4033e30.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
>现在修复bug，需要把“Git is free software ...”改为“Git is a free software ...”，然后提交：
修复完成后，切换到master分支，并完成合并，最后删除issue-101分支：

![上述步骤](https://upload-images.jianshu.io/upload_images/14597179-ca2dc94b23eac05b.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

>1. 现在，接着回到dev分支干活。
>2. 工作区是干净的，刚才的工作现场存到哪去了？用git stash list命令看看：
3>工作现场还在，Git把stash内容存在某个地方了，但是需要恢复一下，有两个办法：一是用`git stash apply`恢复，但是恢复后，stash内容并不删除，你需要用`git stash drop`来删除；另一种方式是用`git stash pop`，恢复的同时把stash内容也删了：

![恢复现场（当然BUG也在）](https://upload-images.jianshu.io/upload_images/14597179-2f9da50af30f617a.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
再用`git stash list`查看，就看不到任何stash内容了：

![image.png](https://upload-images.jianshu.io/upload_images/14597179-5738c72bf1a16a0f.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
**可以多次stash，恢复的时候，先用`git stash list`查看，然后恢复指定的stash，用命令：`git stash apply stash@{0}`
