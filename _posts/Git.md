# Git

## 基础知识

[廖雪峰git教程](https://www.liaoxuefeng.com/wiki/896043488029600)

[状态转换图来源](https://www.cnblogs.com/gavincoder/p/9073368.html)

### Git代码状态

![代码状态转换图](/home/wuqiushi/.config/Typora/typora-user-images/image-20201209221438732.png)

### Git基本操作

创建版本库

git初步

[操作视频](详见百度网盘)

```c
git init
```

查看仓库当前的状态

```c
git status
```

查看具体修改内容

```c
git diff
```



```c
git add file
```

```c
git checkout file
```

```c
git commit -m "更新说明"
```

显示提交日志

```c
git log (--pretty=oneline) //参数缩短显示信息
```

版本回退（指针移动）

```c
git reset (--hard) HEAD^/某个版本号
```

查看命令历史

```c
git relog
```



```c
git checkout 版本号
```

切换分支

```c
git checkout 分支
```

建立分支

```c
git checkout -b 分支名
```

查看分支

```c
git branch
```

以下指令会生成一个.git文件

```c
git clone --bare 路径
```

```c
git remote
git remote add 远程仓库名 .git路径
```

进入另一个文件目录

```c
git clone .git文件路径
```

```c
git remote (show) (orgin) //括号表示可以有可以没有会有提示
```

删除文件

- 命令`git rm`删掉，并且`git commit`：

```c
git rm file
git commit -m ""
```

- 删错了，把误删的文件恢复到最新版本

```c
git checkout -- file
```



### 工作区和暂存区

- 工作区：电脑能看到的目录
- 版本库：隐藏目录 .git

Git的版本库里存了很多东西，其中最重要的就是称为stage（或者叫index）的暂存区，还有Git为我们自动创建的第一个分支`master`，以及指向`master`的一个指针叫`HEAD`。

![工作区暂存区图解](/home/wuqiushi/.config/Typora/typora-user-images/image-20201210000549772.png)

第一步是用`git add`把文件添加进去，实际上就是把文件修改添加到暂存区；

第二步是用`git commit`提交更改，实际上就是把暂存区的所有内容提交到当前分支。

因为我们创建Git版本库时，Git自动为我们创建了唯一一个`master`分支，所以，现在，`git commit`就是往`master`分支上提交更改。

你可以简单理解为，需要提交的文件修改通通放到暂存区，然后，一次性提交暂存区的所有修改。



撤销修改

命令`git checkout -- readme.txt`意思就是，把`readme.txt`文件在工作区的修改全部撤销，这里有两种情况：

一种是`readme.txt`自修改后还没有被放到暂存区，现在，撤销修改就回到和版本库一模一样的状态；

一种是`readme.txt`已经添加到暂存区后，又作了修改，现在，撤销修改就回到添加到暂存区后的状态。

总之，就是让这个文件回到最近一次`git commit`或`git add`时的状态。

### 远程仓库

本地Git仓库和GitHub仓库之间的传输是通过SSH加密的，所以，需要一点设置：

第1步：创建SSH Key。在用户主目录下，看看有没有.ssh目录，如果有，再看看这个目录下有没有`id_rsa`和`id_rsa.pub`这两个文件，如果已经有了，可直接跳到下一步。如果没有，打开Shell（Windows下打开Git Bash），创建SSH Key：

```
$ ssh-keygen -t rsa -C "youremail@example.com"
```

![image-20201210141347950](/home/wuqiushi/.config/Typora/typora-user-images/image-20201210141347950.png)

你需要把邮件地址换成你自己的邮件地址，然后一路回车，使用默认值即可，由于这个Key也不是用于军事目的，所以也无需设置密码。

如果一切顺利的话，可以在用户主目录里找到`.ssh`目录，里面有`id_rsa`和`id_rsa.pub`两个文件，这两个就是SSH Key的秘钥对，`id_rsa`是私钥，不能泄露出去，`id_rsa.pub`是公钥，可以放心地告诉任何人。

第2步：登陆GitHub，打开“Account settings”，“SSH Keys”页面：

然后，点“Add SSH Key”，填上任意Title，在Key文本框里粘贴`id_rsa.pub`文件的内容：

![github-addkey-1](https://www.liaoxuefeng.com/files/attachments/919021379029408/0)

点“Add Key”，你就应该看到已经添加的Key：

![github-addkey-2](https://www.liaoxuefeng.com/files/attachments/919021395420160/0)

添加远程库

### 分支

创建分支并切换

```c
git checkout -b 分支名
```

合并指定分支到当前分支

```c
git merge dev//把dev合并到当前分支
```

删除分支

```c
git branch -d 分支名
```

创建并切换到新的分支

```c
git switch -c 分支名
```

切换分支

```c
git switch 分支名
```

![image-20201210143939426](/home/wuqiushi/.config/Typora/typora-user-images/image-20201210143939426.png)

查看分支合并图

```c
git log --graph
```

强行删除分支

```c
git branch -D 分支名
```



多人协作

从远程仓库克隆时，实际上Git自动把本地的`master`分支和远程的`master`分支对应起来了，并且，远程仓库的默认名称是`origin`

查看远程库的信息

``` c
git remote (-v) //-v显示更详细信息
```

显示了可以抓取和推送的`origin`的地址。如果没有推送权限，就看不到push的地址。



推送分支

推送分支，就是把该分支上的所有本地提交推送到远程库。推送时，要指定本地分支，这样，Git就会把该分支推送到远程库对应的远程分支上：

```
git push origin master
```

上面的指令的原型是

```c
git push origin master:
```



如果要推送其他分支，比如`dev`，就改成：

```
git push origin dev
```



抓取分支



多人协作的工作模式通常是这样：

1. 首先，可以试图用`git push origin `推送自己的修改；
2. 如果推送失败，则因为远程分支比你的本地更新，需要先用`git pull`试图合并；
3. 如果合并有冲突，则解决冲突，并在本地提交；
4. 没有冲突或者解决掉冲突后，再用`git push origin `推送就能成功！

如果`git pull`提示`no tracking information`，则说明本地分支和远程分支的链接关系没有创建，用命令`git branch --set-upstream-to  origin/`。



### 实际应用场景



在开发中将功能分块，主干分支开分支dev(用于测试)，从dev中引出多个分支用于开发，开发好后提交到dev

如双十一如果进行淘宝的重新安装，就是在主分支中引出feature分支供使用

“借尸还魂”现象：远程仓库停用后重建一个同名仓库，本地仓库不知道远程仓库停用的情况下还在提交

