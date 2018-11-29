#小结
1. 当Git无法自动合并分支时，就必须首先解决冲突。解决冲突后，再提交，合并完成。
2. 解决冲突就是把Git合并失败的文件手动编辑为我们希望的内容，再提交。
3. 用git log --graph命令可以看到分支合并图。

#正文

**有两个分支，master 和 feature1分别有新的提交：**
![](https://upload-images.jianshu.io/upload_images/14597179-3edaa975e99dd4ed.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
Git无法执行“快速合并”，只能试图把各自的修改合并起来，但这种合并就可能会有冲突：![](https://upload-images.jianshu.io/upload_images/14597179-448e94dedfe9d19c.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
readme.md文件存在冲突，必须手动解决冲突后再提交。`git status`也可以冲突显示文件：
![](https://upload-images.jianshu.io/upload_images/14597179-05ef6a2b2b483f46.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
查看readme.md文件：
![readme.md](https://upload-images.jianshu.io/upload_images/14597179-7842f42d4041f9bb.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
Git用<<<<<<<，=======，>>>>>>>标记出不同分支的内容
修改然后提交
![修改后的readme.md](https://upload-images.jianshu.io/upload_images/14597179-a290b88508db6a30.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

![重新提交readme.md](https://upload-images.jianshu.io/upload_images/14597179-caead914eec9d86b.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
现在，`master`分支和`feature1`分支变成了下图所示：

![git-br-conflict-merged](http://upload-images.jianshu.io/upload_images/14597179-2d83e968a2ca3ae9?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

用带参数的`git log`也可以看到分支的合并情况：

```
$  git log --graph --pretty=oneline --abbrev-commit
*   8306d83 (HEAD -> master) conflict fixed
|\
| * 6595182 (feature1) branch test
* | f239080 & simple
|/
* b1f5a38 branch test
* 4046b17 (origin/master, origin/HEAD) add test.txt
* d730a19 append GPL
* 8fd1e66 add distributed
* 123e316 wrote a readme file

```

最后，删除`feature1`分支：

```
$ git branch -d feature1
Deleted branch feature1 (was 14096d0).
```
工作完成。
