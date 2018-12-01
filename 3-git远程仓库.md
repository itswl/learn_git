Git是分布式版本控制系统，同一个Git仓库，可以分布到不同的机器上。一般找一台电脑充当服务器的角色，每天24小时开机，其他每个人都从这个“服务器”仓库克隆一份到自己的电脑上，并且各自把各自的提交推送到服务器仓库里，也从服务器仓库中拉取别人的提交。**也可使用github远程仓库**
Git仓库和GitHub仓库之间的传输是通过SSH加密的，需要一点设置：

**第1步：创建SSH Key。在用户主目录下，看看有没有.ssh目录，如果有，再看看这个目录下有没有id_rsa和id_rsa.pub这两个文件，如果已经有了，可直接跳到下一步。如果没有，打开Shell（Windows下打开Git Bash），创建SSH Key：**

```
$ ssh-keygen -t rsa -C "imwl@live.com"
```
把邮件地址换成自己的邮件地址，然后一路回车，使用默认值即可。

如果一切顺利的话，可以在用户主目录里找到.ssh目录，里面有id_rsa和id_rsa.pub两个文件，这两个就是SSH Key的秘钥对，id_rsa是私钥，不能泄露出去，id_rsa.pub是公钥，可以放心地告诉任何人。

**第2步：登陆GitHub，打开“Account settings”，“SSH Keys”页面：**

然后，点“Add SSH Key”，填上任意Title，在Key文本框里粘贴id_rsa.pub文件的内容：
![github-addkey-1](http://upload-images.jianshu.io/upload_images/14597179-10901f2c6cdb2f6a?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

点“Add Key”，你就应该看到已经添加的Key：

![github-addkey-2](http://upload-images.jianshu.io/upload_images/14597179-63e04ca796774279?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

目前，在GitHub上的这个learn_git仓库还是空的
![learn_git](https://upload-images.jianshu.io/upload_images/14597179-e3d5304580aa5ea7.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
在本地的git仓库下运行命令：
`git remote add origin git@github.com:itswl/learn_git.git`
添加后，远程库的名字就是origin，这是Git默认的叫法，也可以改成别的
（如果下面失败可以在此`git pull origin master`）
下一步，就可以把本地库的所有内容推送到远程库上` git push -u origin master`：
```
$ git push -u origin master
Counting objects: 20, done.
Delta compression using up to 4 threads.
Compressing objects: 100% (15/15), done.
Writing objects: 100% (20/20), 1.64 KiB | 560.00 KiB/s, done.
Total 20 (delta 5), reused 0 (delta 0)
remote: Resolving deltas: 100% (5/5), done.
To github.com:michaelliao/learngit.git
 * [new branch]      master -> master
Branch 'master' set up to track remote branch 'master' from 'origin'.
```
把本地库的内容推送到远程，用git push命令，实际上是把当前分支master推送到远程。

由于远程库是空的，我们第一次推送master分支时，加上了-u参数，Git不但会把本地的master分支内容推送的远程新的master分支，还会把本地的master分支和远程的master分支关联起来，在以后的推送或者拉取时就可以简化命令。（接上面第一次可能要用`git push -u origin master -f `强制push）
推送成功后，可以立刻在GitHub页面中看到远程库的内容已经和本地一模一样：
![github与本地一样](https://upload-images.jianshu.io/upload_images/14597179-d8885543f3f7fafe.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)


从现在起，只要本地作了提交，就可以通过命令：

```
$ git push origin master
```

把本地`master`分支的最新修改推送至GitHub
将本地readme.md 和test.txt删除

**从远程库克隆**
用命令git clone克隆到本地库` git clone git@github.com:itswl/learn_git`
```
imwl@DESKTOP-V2KTJSJ MINGW64 /d/WeiLai/OneDrive/study
$ git clone git@github.com:itswl/learn_git
Cloning into 'learn_git'...
remote: Enumerating objects: 12, done.
remote: Counting objects: 100% (12/12), done.
remote: Compressing objects: 100% (7/7), done.
remote: Total 12 (delta 1), reused 12 (delta 1), pack-reused 0
Receiving objects: 100% (12/12), done.
Resolving deltas: 100% (1/1), done.
```
就可以看到study文件夹中多了一个learn_git文件夹。
![与giuhub中相同](https://upload-images.jianshu.io/upload_images/14597179-c4ef5cbdf39954eb.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
