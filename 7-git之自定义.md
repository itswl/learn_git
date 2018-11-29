



实际上，Git还有很多可配置项。
##让Git显示颜色，会让命令输出看起来更醒目：

```
$ git config --global color.ui true
```
这样，Git会适当地显示不同的颜色，比如`git status`命令：

![git-color](http://upload-images.jianshu.io/upload_images/14597179-328e7d67fd85390c?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

文件名就会标上颜色。
##忽略特殊文件
某些文件放到Git工作目录中，但又不能提交它们，比如保存了数据库密码的配置文件等，每次`git status`都会显示`Untracked files ...`

**在Git工作区的根目录下创建一个特殊的`.gitignore`文件，然后把要忽略的文件名填进去，Git就会自动忽略这些文件。**

不需要从头写`.gitignore`文件，GitHub已经准备了各种配置文件，只需要组合一下就可以使用了。所有配置文件可以直接在线浏览：https://github.com/github/gitignore

忽略文件的原则是：

>1.  忽略操作系统自动生成的文件，比如缩略图等；
>2.  忽略编译生成的中间文件、可执行文件等，也就是如果一个文件是通过另一个文件自动生成的，那自动生成的文件就没必要放进版本库，比如Java编译产生的`.class`文件；
>3.  忽略你自己的带有敏感信息的配置文件，比如存放口令的配置文件。

举个例子：

假设你在Windows下进行Python开发，Windows会自动在有图片的目录下生成隐藏的缩略图文件，如果有自定义目录，目录下就会有`Desktop.ini`文件，因此你需要忽略Windows自动生成的垃圾文件：

```
# Windows:
Thumbs.db
ehthumbs.db
Desktop.ini

```

然后，继续忽略Python编译产生的`.pyc`、`.pyo`、`dist`等文件或目录：

```
# Python:
*.py[cod]
*.so
*.egg
*.egg-info
dist
build

```

加上你自己定义的文件，最终得到一个完整的`.gitignore`文件，内容如下：

```
# Windows:
Thumbs.db
ehthumbs.db
Desktop.ini

# Python:
*.py[cod]
*.so
*.egg
*.egg-info
dist
build

# My configurations:
db.ini
deploy_key_rsa

```

最后一步就是把`.gitignore`也提交到Git，就完成了！当然检验`.gitignore`的标准是`git status`命令是不是说`working directory clean`。

使用Windows的童鞋注意了，如果你在资源管理器里新建一个`.gitignore`文件，它会非常弱智地提示你必须输入文件名，但是在文本编辑器里“保存”或者“另存为”就可以把文件保存为`.gitignore`了。

有些时候，你想添加一个文件到Git，但发现添加不了，原因是这个文件被`.gitignore`忽略了：

```
$ git add App.class
The following paths are ignored by one of your .gitignore files:
App.class
Use -f if you really want to add them.

```

如果你确实想添加该文件，可以用`-f`强制添加到Git：

```
$ git add -f App.class

```

或者你发现，可能是`.gitignore`写得有问题，需要找出来到底哪个规则写错了，可以用`git check-ignore`命令检查：

```
$ git check-ignore -v App.class
.gitignore:3:*.class    App.class

```

Git会告诉我们，`.gitignore`的第3行规则忽略了该文件，于是我们就可以知道应该修订哪个规则。

### 小结

*   忽略某些文件时，需要编写`.gitignore`；

*   `.gitignore`文件本身要放到版本库里，并且可以对`.gitignore`做版本管理！

#配置别名

比如敲`git st`就表示`git status`
```
$ git config --global alias.st status
```
当然还有别的命令可以简写，很多人都用`co`表示`checkout`，`ci`表示`commit`，`br`表示`branch`：
```
$ git config --global alias.co checkout
$ git config --global alias.ci commit
$ git config --global alias.br branch
```

以后提交就可以简写成：

```
$ git ci -m "bala bala bala..."
```
**`--global`参数是全局参数，也就是这些命令在这台电脑的所有Git仓库下都有用。**

命令`git reset HEAD file`可以把暂存区的修改撤销掉（unstage），重新放回工作区。既然是一个unstage操作，就可以配置一个`unstage`别名：
```
$ git config --global alias.unstage 'reset HEAD'
```
当你敲入命令：
```
$ git unstage test.py
```
实际上Git执行的是：
```
$ git reset HEAD test.py
```

配置一个`git last`，让其显示最后一次提交信息：

```
$ git config --global alias.last 'log -1'
```
这样，用`git last`就能显示最近一次的提交：

```
$ git last
commit adca45d317e6d8a4b23f9811c3d7b7f0f180bfe2
Merge: bd6ae48 291bea8
Author: Michael Liao <askxuefeng@gmail.com>
Date:   Thu Aug 22 22:49:22 2013 +0800

    merge & fix hello.py

```
甚至还有人丧心病狂地把`lg`配置成了：
```
git config --global alias.lg "log --color --graph --pretty=format:'%Cred%h%Creset -%C(yellow)%d%Creset %s %Cgreen(%cr) %C(bold blue)<%an>%Creset' --abbrev-commit"
```
来看看`git lg`的效果：
![git-lg](http://upload-images.jianshu.io/upload_images/14597179-8a4705577f38c01f?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
为什么不早点告诉我？别激动，咱不是为了多记几个英文单词嘛！

# 配置文件

配置Git的时候，加上`--global`是针对当前用户起作用的，如果不加，那只针对当前的仓库起作用。

配置文件放哪了？每个仓库的Git配置文件都放在`.git/config`文件中：

```
$ cat .git/config 
[core]
    repositoryformatversion = 0
    filemode = true
    bare = false
    logallrefupdates = true
    ignorecase = true
    precomposeunicode = true
[remote "origin"]
    url = git@github.com:michaelliao/learngit.git
    fetch = +refs/heads/*:refs/remotes/origin/*
[branch "master"]
    remote = origin
    merge = refs/heads/master
[alias]
    last = log -1

```

别名就在`[alias]`后面，要删除别名，直接把对应的行删掉即可。

而当前用户的Git配置文件放在用户主目录下的一个隐藏文件`.gitconfig`中：

```
$ cat .gitconfig
[alias]
    co = checkout
    ci = commit
    br = branch
    st = status
[user]
    name = Your Name
    email = your@email.com

```

配置别名也可以直接修改这个文件，如果改错了，可以删掉文件重新通过命令配置。

### 小结

给Git配置好别名，就可以输入命令时偷个懒。
