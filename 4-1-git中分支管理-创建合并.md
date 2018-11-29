#小结
Git鼓励大量使用分支：
查看分支：`git branch`
创建分支：`git branch <name>` eg:`git branch dev`
切换分支：`git checkout <name>` eg:`git checkout dev`
创建+切换分支：`git checkout -b <name>` eg:` git checkout -b dev`(相当于上面两条命令）
合并某分支到当前分支：`git merge <name>` eg:`git merge dev`
删除分支：`git branch -d <name>`  eg:`git branch -d dev`

#正文
**在Git里，master分支叫主分支。**
一开始的时候，`master`分支是一条线，Git用`master`指向最新的提交，再用`HEAD`指向`master`，就能确定当前分支，以及当前分支的提交点：

![git-br-initial](http://upload-images.jianshu.io/upload_images/14597179-a9c04e64b6ff65ac?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

每次提交，`master`分支都会向前移动一步，这样，随着你不断提交，`master`分支的线也越来越长：

当我们创建新的分支，例如`dev`时，Git新建了一个指针叫`dev`，指向`master`相同的提交，再把`HEAD`指向`dev`，就表示当前分支在`dev`上：

![git-br-create](http://upload-images.jianshu.io/upload_images/14597179-366bce6dbbfe994a?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

你看，Git创建一个分支很快，因为除了增加一个`dev`指针，改改`HEAD`的指向，工作区的文件都没有任何变化！

不过，从现在开始，对工作区的修改和提交就是针对`dev`分支了，比如新提交一次后，`dev`指针往前移动一步，而`master`指针不变：

![git-br-dev-fd](http://upload-images.jianshu.io/upload_images/14597179-02fb9ede79fcd202?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

假如我们在`dev`上的工作完成了，就可以把`dev`合并到`master`上。Git怎么合并呢？最简单的方法，就是直接把`master`指向`dev`的当前提交，就完成了合并：

![git-br-ff-merge](http://upload-images.jianshu.io/upload_images/14597179-7b53bb949edf3c76?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

所以Git合并分支也很快！就改改指针，工作区内容也不变！

合并完分支后，甚至可以删除`dev`分支。删除`dev`分支就是把`dev`指针给删掉，删掉后，我们就剩下了一条`master`分支：

![git-br-rm](http://upload-images.jianshu.io/upload_images/14597179-d24d4eaddb364f34?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
