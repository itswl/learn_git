#小结
1. 工作区（Working Directory)就是电脑里能看到的目录。
2. 版本库（Repository）工作区有一个隐藏目录`.git`，这个不算工作区，而是Git的版本库。
3. `git add`实际上就是把文件修改添加到暂存区，`git commit`实际上就是把暂存区的所有内容提交到当前分支。
4. `git checkout -- file`丢弃工作区的修改。
5. `git reset HEAD <file>`把暂存区的修改撤销掉（unstage），重新放回工作区。
6. 从版本库中删除该文件，那就用命令`git rm`删掉，并且`git commit`
7. 把误删的文件恢复到版本库的版本`git checkout`

#正文

#### 工作区（Working Directory）

就是电脑里能看到的目录，比如`git`文件夹就是一个工作区：

![工作区（Working Directory）](https://upload-images.jianshu.io/upload_images/14597179-37177327fcf2fe92.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)



#### 版本库（Repository）

工作区有一个隐藏目录`.git`，这个不算工作区，而是Git的版本库。
![隐藏目录.git](https://upload-images.jianshu.io/upload_images/14597179-a4effd3b3fb5ec5b.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

![Git版本库](https://upload-images.jianshu.io/upload_images/14597179-1018795b03191282.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

Git的版本库里存了很多东西，其中最重要的就是称为stage（或者叫index）的暂存区，还有Git为我们自动创建的第一个分支`master`，以及指向`master`的一个指针叫`HEAD`。


![git-repo](http://upload-images.jianshu.io/upload_images/14597179-cc335edbf830ab71?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

分支和`HEAD`的概念我们以后再讲。

前面讲了我们把文件往Git版本库里添加的时候，是分两步执行的：

第一步是用`git add`把文件添加进去，实际上就是把文件修改添加到暂存区；

第二步是用`git commit`提交更改，实际上就是把暂存区的所有内容提交到当前分支。

因为我们创建Git版本库时，Git自动为我们创建了唯一一个`master`分支，所以，现在，`git commit`就是往`master`分支上提交更改。

你可以简单理解为，需要提交的文件修改通通放到暂存区，然后，一次性提交暂存区的所有修改。

俗话说，实践出真知。现在，我们再练习一遍，先对`readme.txt`做个修改，比如加上一行内容：

```
Git is a distributed version control system.
Git is free software distributed under the GPL.
Git has a mutable index called stage.

```

然后，在工作区新增一个`LICENSE`文本文件（内容随便写）。

先用`git status`查看一下状态：

```
$ git status
On branch master
Changes not staged for commit:
  (use "git add <file>..." to update what will be committed)
  (use "git checkout -- <file>..." to discard changes in working directory)

    modified:   readme.txt

Untracked files:
  (use "git add <file>..." to include in what will be committed)

    LICENSE

no changes added to commit (use "git add" and/or "git commit -a")

```

Git非常清楚地告诉我们，`readme.txt`被修改了，而`LICENSE`还从来没有被添加过，所以它的状态是`Untracked`。

现在，使用两次命令`git add`，把`readme.txt`和`LICENSE`都添加后，用`git status`再查看一下：

```
$ git status
On branch master
Changes to be committed:
  (use "git reset HEAD <file>..." to unstage)

    new file:   LICENSE
    modified:   readme.txt

```

现在，暂存区的状态就变成这样了：

![git-stage](http://upload-images.jianshu.io/upload_images/14597179-95ddbacf92ccd6b7?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

所以，`git add`命令实际上就是把要提交的所有修改放到暂存区（Stage），然后，执行`git commit`就可以一次性把暂存区的所有修改提交到分支。

```
$ git commit -m "understand how stage works"
[master e43a48b] understand how stage works
 2 files changed, 2 insertions(+)
 create mode 100644 LICENSE

```

一旦提交后，如果你又没有对工作区做任何修改，那么工作区就是“干净”的：

```
$ git status
On branch master
nothing to commit, working tree clean

```

现在版本库变成了这样，暂存区就没有任何内容了：

![git-stage-after-commit](http://upload-images.jianshu.io/upload_images/14597179-83d4fda49d011a61?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)


**Git跟踪并管理的是修改，而非文件。每次修改，如果不用`git add`到暂存区，那就不会加入到`commit`中**
**第一次修改 -> ``git add`` -> 第二次修改 -> ``git commit``**,只会管理到第一次修改。
**可以多次``git add``后一并``git commit``:**
**第一次修改 -> ``git add`` -> 第二次修改 -> ``git add`` ->`` git commit``**
**`git checkout -- file`可以丢弃工作区的修改:**
``git checkout -- readme.md``，就是让这个文件回到最近一次git commit或git add时的状态。
**如果已经git add,但没有git commit,可以通过命令`git reset HEAD <file>`可以把暂存区的修改撤销掉（unstage），重新放回工作区。例如`git reset HEAD readme.md`**
`git reset`命令既可以回退版本，也可以把暂存区的修改回退到工作区。当我们用HEAD时，表示最新的版本。

已经提交了不合适的修改到版本库时，想要撤销本次提交，可以版本回退，不过前提是没有推送到远程库

###文件删除
新建一个test.txt文件到Git并提交，`git add test.txt`,`git commit -m "add test.txt"`。本地删除后，Git知道你删除了文件，因此，工作区和版本库就不一致了，git status命令会立刻告诉你哪些文件被删除了：
```
$ git status
On branch master
Changes not staged for commit:
  (use "git add/rm <file>..." to update what will be committed)
  (use "git checkout -- <file>..." to discard changes in working directory)

    deleted:    test.txt

no changes added to commit (use "git add" and/or "git commit -a")
```
现在你有两个选择:
**1. 确实要从版本库中删除该文件，那就用命令`git rm`删掉，并且`git commit`：**
```
$ git rm test.txt
rm 'test.txt'

$ git commit -m "remove test.txt"
[master d46f35e] remove test.txt
 1 file changed, 1 deletion(-)
 delete mode 100644 test.txt
```
**2. 删错了，版本库里还有，所以可以很轻松地把误删的文件恢复到版本库的版本：**
```
$ git checkout -- test.txt
```
`git checkout`其实是用版本库里的版本替换工作区的版本

