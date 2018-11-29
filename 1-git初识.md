#小结

1. 初始化一个Git仓库，使用git init命令。

2. 添加文件到Git仓库，分两步：

使用命令`git add <file>`，注意，可反复多次使用，添加多个文件；
使用命令`git commit -m <message>`，完成。
3. 要随时掌握工作区的状态，使用`git status`命令。

4. 如果git status告诉你有文件被修改过，用`git diff`可以查看修改内容。

5. HEAD指向的版本就是当前版本，因此，Git允许我们在版本的历史之间穿梭，使用命令`git reset --hard commit_id`。

6. 穿梭前，用`git log`可以查看提交历史，以便确定要回退到哪个版本。

7. 要重返未来，用`git reflog`查看命令历史，以便确定要回到未来的哪个版本。

#正文

参照廖雪峰的git教程学习，感谢廖雪峰老师的教程。
教程地址：https://www.liaoxuefeng.com/wiki/0013739516305929606dd18361248578c67b8067c8c017b000

git是分布式版本控制系统之一，直接在官网上下载安装

进入目录 比如`d/WeiLai/Onedrive/study/git`,然后`git bash here`,输入`pwd`显示当前目录。

输入 `git init `命令把这个目录变成Git可以管理的仓库
提示`Initialized empty Git repository in D:/WeiLai/OneDrive/study/git/.git/`
当前目录下多了一个.git的目录，这个目录是Git来跟踪管理版本库的
这个目录默认是隐藏的，用`ls -ah`命令就可以看见

目录中新建一个文件 readme.md ,内容如下:

>Git is a version control system.
>Git is free software.

`git add readme.md` 用git add把文件添加到仓库
`git commit -m "wrote a readme file"`用命令git commit把文件提交到仓库("wrote a readme file"本次提交的说明）

**为什么Git添加文件需要add，commit一共两步呢？**
因为commit可以一次提交很多文件，所以你可以多次add不同的文件，比如：

`git add file1.txt`
` git add file2.txt file3.txt`
` git commit -m "add 3 files."`

对readme.md进行修改如下:
>Git is a distributed version control system.
>Git is free software.

修改readme.md后，输入`git status`命令可以让我们时刻掌握仓库当前的状态。(可以查看到readme.md被修改但未提交。)

使用 git diff命令查看具体修改内容，`git diff readme.md` 

查看完后（同前面），可通过git add(添加) ,git status(查看当前状态),git committ(提交)。

再次对readme.md进行如下修改:

>Git is a distributed version control system.
Git is free software distributed under the GPL.

然后尝试提交：
```
$ git add readme.md
$ git commit -m "append GPL"
[master 1094adb] append GPL
 1 file changed, 1 insertion(+), 1 deletion(-)
```

#####readme.md文件一共有3个版本被提交到Git仓库里了：

**版本1：wrote a readme file**
>Git is a version control system.
Git is free software.

**版本2：add distributed**
>Git is a distributed version control system.
>Git is free software.

**版本3：append GPL**
>Git is a distributed version control system.
>Git is free software distributed under the GPL.

**也用`git log`命令查看历史记录(git log命令显示从最近到最远的提交日志)**
```
imwl@DESKTOP-V2KTJSJ MINGW64 /d/WeiLai/Onedrive/study/git (master)
$ git log
commit d730a1998de2ac8a90b24482563756eb9b400b88 (HEAD -> master)
Author: itswl <imwl@live.com>
Date:   Mon Nov 5 19:14:32 2018 +0800

    append GPL

commit 8fd1e66ce9631da4fea19205b4f737abcb81f059
Author: itswl <imwl@live.com>
Date:   Mon Nov 5 18:47:48 2018 +0800

    add distributed

commit 123e3165aafcea2a49c050beba5f9d2b0099a2ea
Author: itswl <imwl@live.com>
Date:   Mon Nov 5 18:29:10 2018 +0800

    wrote a readme file
```
**使用git log --pretty=oneline只显示commit id 和提交说明**
```
imwl@DESKTOP-V2KTJSJ MINGW64 /d/WeiLai/Onedrive/study/git (master)
$ git log --pretty=oneline
d730a1998de2ac8a90b24482563756eb9b400b88 (HEAD -> master) append GPL
8fd1e66ce9631da4fea19205b4f737abcb81f059 add distributed
123e3165aafcea2a49c050beba5f9d2b0099a2ea wrote a readme file
```

在Git中，用HEAD表示当前版本，也就是最新的提交1094adb...（注意我的提交ID和你的肯定不一样），上一个版本就是HEAD\^，上上一个版本就是HEAD\^\^，往上100个版本写100个^比较容易数不过来，所以写成HEAD~100

**当前版本`append GPL`回退到上一个版本`add distributed`，可以使用`git reset`命令：**
```
$ git reset --hard HEAD^
HEAD is now at 8fd1e66 add distributed
```
readme.md已被还原成第2个版本
```
$ cat readme.md
Git is a distributed version control system.
Git is free software.
```
**git log再看看现在版本库的状态**
```
commit 8fd1e66ce9631da4fea19205b4f737abcb81f059 (HEAD -> master)
Author: itswl <imwl@live.com>
Date:   Mon Nov 5 18:47:48 2018 +0800

    add distributed

commit 123e3165aafcea2a49c050beba5f9d2b0099a2ea
Author: itswl <imwl@live.com>
Date:   Mon Nov 5 18:29:10 2018 +0800

    wrote a readme file
```
**第3个版本已经没有了，找到那个append GPL的commit id是d730a...，于是就可以指定回到未来的某个版本：**
```
$ git reset --hard d730a
HEAD is now at d730a19 append GPL 
```
版本号不用写全，前几位就可以了，Git会自动去找。当然也不能只写前一两位，因为Git可能会找到多个版本号，就无法确定是哪一个了。

再查看readme.md的内容：
```
$ cat readme.md
Git is a distributed version control system.
Git is free software distributed under the GPL
```
Git的版本回退速度非常快，因为Git在内部有个指向当前版本的`HEAD`指针，当你回退版本的时候，Git仅仅是把HEAD从指向`append GPL`：

![git-head](http://upload-images.jianshu.io/upload_images/14597179-5f8a05a5bb5eda91?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

改为指向`add distributed`：

![git-head-move](http://upload-images.jianshu.io/upload_images/14597179-4b94ce0352477016?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

然后顺便把工作区的文件更新了。所以你让`HEAD`指向哪个版本号，你就把当前版本定位在哪。

当你用` git reset --hard HEAD^`回退到`add distributed`版本时，再想恢复到`append GPL`，就必须找到`append GPL`的`commit id`。Git提供了一个命令`git reflog`用来记录你的每一次命令：
```
imwl@DESKTOP-V2KTJSJ MINGW64 /d/WeiLai/Onedrive/study/git (master)
$ git reflog
d730a19 (HEAD -> master) HEAD@{0}: reset: moving to d730a
8fd1e66 HEAD@{1}: reset: moving to HEAD^
d730a19 (HEAD -> master) HEAD@{2}: commit: append GPL
8fd1e66 HEAD@{3}: commit: add distributed
123e316 HEAD@{4}: commit (initial): wrote a readme file
```
