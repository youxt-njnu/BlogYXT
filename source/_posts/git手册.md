---
title: git手册
date: 2023-05-17 10:50:16
tags:
 - git
 - 设备迁移
categories:
 - 版本控制
comments: true
cover: https://cdn.jsdelivr.net/gh/youxt-njnu/blog-img/20230511202603.png
photos: https://cdn.jsdelivr.net/gh/youxt-njnu/blog-img/20230511202603.png
---

# 版本控制

用来记录文件变化，以便查阅特定版本修订情况；

把手工管理文件版本的方式，改为由软件管理文件的版本；而负责管理文件版本的软件就叫做版本控制软件

优点：

> 操作简便
> 
> 易于对比
> 
> 易于回溯
> 
> 不易丢失
> 
> 协作方便

分类：

* 本地版本控制系统
* 集中化的版本控制系统：如SVN；服务器保存文件的所有记录，客户端只保留最新的文件版本
* 分布式版本控制系统：如git；服务器保存文件所有最新版本，客户端上保存了服务器的完整备份

git特性：

* 直接记录快照（类似备份），而非差异比较
* 近乎所有操作都是本地执行

git管理项目的三个区域

* 工作区
* 暂存区
* git仓库

git中的三种状态：

* modified，已修改
* staged，已暂存
* committed，已提交

基本git工作流：

1. 在工作区中修改文件
2. 将想要下次提交的更改进行暂存
3. 提交更新，将快照永久存储到git仓库中

开源许可协议open source license：

* BSD(berkeley software distribution)
* Apache Licence 2.0
* GPL(GNU general public license)
  * 具有传染性，不允许修改后和衍生的代码作为闭源的商业软件发布和销售
  * 如LInux
* LGPL(GNU lesser general public license)
* MIT(massachesetts institute of technology, MIT)
  * 在修改的代码或者发行包中，必须包含原作者的许可信息，对商业友好
  * JQuery, Node.js, 多前端协议

开源项目托管平台，专门用于免费存放开源项目源代码的网站，三个用git管理的平台：

* github, 全球最牛
* gitlab, 对代码私有性支持较好，企业用户较多
* gitee, 码云，国产

![](https://cdn.jsdelivr.net/gh/youxt-njnu/blog-img/git%E6%89%8B%E5%86%8C-1.png)

![](https://cdn.jsdelivr.net/gh/youxt-njnu/blog-img/git%E6%89%8B%E5%86%8C-4.png)

![](https://cdn.jsdelivr.net/gh/youxt-njnu/blog-img/git%E6%89%8B%E5%86%8C-5.png)

# 具体内容

**设置用户名和邮件地址，区分操作人的信息**

> --global 该命令一次使用，永久生效

git config --global user.name 用户名 设置用户签名 
git config --global user.email 邮箱 设置用户签名 

> 这些信息会更新到c:/users/用户文件夹/.gitconfig文件（配置文件）中

```
$ git config --global user.name Layne 
Layne@LAPTOP-Layne MINGW64 /d/Git-Space/SH0720 (master) 
$ git config --global user.email Layne@atguigu.com 
Layne@LAPTOP-Layne MINGW64 /d/Git-Space/SH0720 (master) 
$ cat ~/.gitconfig 
[user] 
 name = Layne 
 email = Layne@atguigu.com

#  查看所有的全局配置项
$ git config --list --global
# 查看指定的全局配置项
$ git config user.name
$ git config user.email

# 获取帮助信息 git help <verb>, git <verb> -h
$ git help config
$ git config -h
```

**将本地未进行版本控制的目录转换为git仓库**

git init 初始化本地库 

> 会创建一个名为.git的隐藏目录，包含了初始的必要文件

**查看本地库状态**

git status

> 工作区的4中状态：
> 
> untracked(不被git管理), unmodified(工作区和git仓库的文件内容一致), modified(不一致), staged(工作区中修改内容已放在暂存区，准备将修改后的文件保存到git仓库)
> 
> 以精简状态显示文件状态报告：(--short)
> 
> git status -s
> 
> 此时未跟踪的文件前面有红色的??标记，修改了的文件前面有红色的M

**添加到暂存区**

git add 文件名 

> 此时文件已被跟踪，添加到了暂存区，git status -s下这个文件前面有绿色A的标记
> 
> 若提交的是修改的已有文件，会显示绿色的M

**对所有文件暂存**

> `git add xx`命令可以将xx文件添加到暂存区，如果有很多改动可以通过 `git add -A .`来一次添加所有改变的文件。
> 
> 注意 `-A` 选项后面还有一个句点。 `git add -A`表示添加所有内容， `git add .` 表示添加新文件和编辑过的文件不包括删除的文件; `git add -u` 表示添加编辑或者删除的文件，不包括新添加的文件

```
$ git add hello.txt 
warning: LF will be replaced by CRLF in hello.txt. 
The file will have its original line endings in your working 
directory. 
```

**提交到本地库**

git commit -m "日志信息" 文件名 

或者不加文件名，表示全部提交

```
$ git commit -m "my first commit" hello.txt 
warning: LF will be replaced by CRLF in hello.txt. 
The file will have its original line endings in your working 
directory. 
[master (root-commit) 86366fa] my first commit 
 1 file changed, 16 insertions(+) 
 create mode 100644 hello.txt
```

**跳过暂存区直接将工作区内容提交到git仓库**

git commit -a -m "日志信息"

**查看提交历史**

git reflog  查看版本信息 

git log  查看版本详细信息，按照提交历史，最近的提交显示在最上面

> :表示下面还有内容，可以点击enter继续查看；
> 
> 也可以点击q然后退出查看
> 
> git log -2可以查看最近两次的提交历史
> 
> git log -2 --pretty=oneline，在一行上显示最近两条消息
> 
> git log -2 --pretty=format:"%h | %an | % ar | %s"，自定义输出格式，%h表示提交的简写哈希值，%an表示作者名字，%ar表示作者修订日期，%s表示提交说明

```
$ git reflog 
087a1a7 (HEAD -> master) HEAD@{0}: commit: my third commit 
ca8ded6 HEAD@{1}: commit: my second commit 
86366fa HEAD@{2}: commit (initial): my first commit
```

**从暂存区中移除对应的文件**

git reset --hard 文件名

**从暂存区中移除所有文件**

git reset --hard .

**版本穿梭**

git reset --hard 版本号

```
# 首先查看当前的历史记录，可以看到当前是在 087a1a7这个版本 
Layne@LAPTOP-Layne MINGW64 /d/Git-Space/SH0720 (master) 
$ git reflog 
087a1a7 (HEAD -> master) HEAD@{0}: commit: my third commit 
ca8ded6 HEAD@{1}: commit: my second commit 
86366fa HEAD@{2}: commit (initial): my first commit 

# 切换到 86366fa版本，也就是我们第一次提交的版本 
Layne@LAPTOP-Layne MINGW64 /d/Git-Space/SH0720 (master) 
$ git reset --hard 86366fa 
HEAD is now at 86366fa my first commit 

# 切换完毕之后再查看历史记录，当前成功切换到了 86366fa版本 
Layne@LAPTOP-Layne MINGW64 /d/Git-Space/SH0720 (master) 
$ git reflog 
86366fa (HEAD -> master) HEAD@{0}: reset: moving to 86366fa 
087a1a7 HEAD@{1}: commit: my third commit 
ca8ded6 HEAD@{2}: commit: my second commit 
86366fa (HEAD -> master) HEAD@{3}: commit (initial): my first commit 

# 然后查看文件 hello.txt，发现文件内容已经变化 
$ cat hello.txt  
hello git! hello atguigu! 
hello git! hello atguigu! 
hello git! hello atguigu! 
hello git! hello atguigu! 
hello git! hello atguigu! 
hello git! hello atguigu! 
hello git! hello atguigu!
```

输入 git push -f命令，将当前的版本的本地仓库推送至远程代码仓库，即可完成远程代码仓库版本回退到第一次提交的版本

```git
Layne@LAPTOP-Layne MINGW64 /d/Git-Space/SH0720 (master) 
$ git push -f 
```

**撤销修改（这次操作危险性高）**

git chckout -- 文件名称

**远程地址设置**

git remote -v 查看当前所有远程地址别名 

git remote add 别名 远程地址 起别名 

```
$ git remote -v 
Layne@LAPTOP-Layne MINGW64 /d/Git-Space/SH0720 (master) 
$ git remote add ori https://github.com/atguiguyueyue/git-shTest.git 

Layne@LAPTOP-Layne MINGW64 /d/Git-Space/SH0720 (master) 
$ git remote -v 
ori     https://github.com/atguiguyueyue/git-shTest.git (fetch) 
ori     https://github.com/atguiguyueyue/git-shTest.git (push)
```

也可以通过下面的命令，查看远程仓库中所有的分支列表信息：

git remote show 远程仓库名称（默认都叫origin）

**分支**

> 一般程序开发中，是不允许在主分支上进行代码修改和操作的，风险太高；
> 
> 程序员在功能分支上进行开发，新功能开发且测试完毕后，再合并到主分支上；

**创建分支**

git branch 分支名

```
$  git branch hot-fix 
Layne@LAPTOP-Layne MINGW64 /d/Git-Space/SH0720 (master) 
$ git branch -v 
  hot-fix 087a1a7 my third commit  （刚创建的新的分支，并将主分支 master
的内容复制了一份） 
* master  087a1a7 my third commit
```

**查看分支**

git branch

git branch -v

```
$ git branch -v 
* master 087a1a7 my third commit  （*代表当前所在的分区）
```

**切换分支**

git checkout 分支名

```
$ git checkout hot-fix 
Switched to branch 'hot-fix' 
--发现当先分支已由 master改为 hot-fix 
Layne@LAPTOP-Layne MINGW64 /d/Git-Space/SH0720 (hot-fix) 
$ 
--查看 hot-fix分支上的文件内容发现与 master分支上的内容不同 
Layne@LAPTOP-Layne MINGW64 /d/Git-Space/SH0720 (hot-fix) 
$ cat hello.txt 
hello git! hello atguigu! 2222222222222 
hello git! hello atguigu! 3333333333333 
hello git! hello atguigu! 
hello git! hello atguigu! 
hello git! hello atguigu!
```

可以创建指定名称的新分支，并立即切换到新分支上，但要注意再master的基础上创建新分支

```
# -b 表示创建一个新分支，checkout表示切换到刚新建的分支上
git checkout -b 分支名称
```

**推送本地分支上的内容到远程仓库**

`git push -u 别名 本地分支名称` # 第一次，-u表示把本地分支和远程分支进行关联，如果push的时候想要修改分支名称，可以`git push -u 别名 本地分支名称:远程分支名称`

git push 别名 分支 # 之后的，可以写上别名和分支，也可以就直接写git push

```
$ git push ori master 
Logon failed, use ctrl+c to cancel basic credential prompt. 
Username for 'https://github.com': atguiguyueyue 
Counting objects: 3, done. 
Delta compression using up to 12 threads. 
Compressing objects: 100% (2/2), done. 
Writing objects: 100% (3/3), 276 bytes | 276.00 KiB/s, done. 
Total 3 (delta 0), reused 0 (delta 0) 
To https://github.com/atguiguyueyue/git-shTest.git 
 * [new branch]      master -> master 
```

**跟踪分支**

从远程仓库中，把远程分支下载到本地仓库中

```
# git checkout 远程分支名称
$ git checkout pay
# 从远程仓库中，把对应的远程分支下载到本地仓库，并对其重命名
# git checkout -b 本地分支名称 远程仓库名称/远程分支名称
$ git checkout -b payment origin/pay
```

**把指定的分支合并到当前分支上**

git merge 分支名 

```
$ git checkout master
# 先切换到主分支，然后将功能分支合并到主分支
$ git merge hot-fix 
Auto-merging hello.txt 
CONFLICT (content): Merge conflict in hello.txt 
Automatic merge failed; fix conflicts and then commit the result.
```

```
$ git status 
On branch master 
You have unmerged paths. 
  (fix conflicts and run "git commit") 
  (use "git merge --abort" to abort the merge) 

Unmerged paths: 
  (use "git add <file>..." to mark resolution) 

        both modified:   hello.txt 

no changes added to commit (use "git add" and/or "git commit -a")

# 编辑有冲突的文件，删除特殊符号，决定要使用的内容 
# 特殊符号：<<<<<<< HEAD 当前分支的代码 =======  合并过来的代码  >>>>>>> hot-fix 

$ git commit -m "merge hot-fix" 
[master 69ff88d] merge hot-fix 
--发现后面 MERGING消失，变为正常 
Layne@LAPTOP-Layne MINGW64 /d/Git-Space/SH0720 (master) 
```

```
# 也可以打开包含冲突的文件，手动解决冲突之后，再执行如下命令
$ git chechout master
$ git merge reg
# 手动解决冲突文件,vscode会智能地提醒你如何进行修改
$ git add .
$ git commit -m "解决了分支合并冲突的问题"
```

**删除分支**

当把功能分支合并到master分支之后，可以删除对应的功能分支：

```
# git branch
git branch -d 功能分支 #要保证现阶段不是在要删除的分支上
```

**移除文件**

从git仓库和工作区中同时移除对应文件：git rm -f 文件名

从git仓库中移除指定文件，但保留工作区中对应的文件：git rm --cached 文件名

**忽略文件**

可以创建一个名为.gitignore的配置文件，列出要忽略的文件的匹配模式

> #开头，注释
> 
> /结尾，目录
> 
> /开头，递归
> 
> !开头，取反
> 
> 可以用glob模式进行文件和文件夹的匹配，glob指简化了的正则表达式
> 
> ![](https://cdn.jsdelivr.net/gh/youxt-njnu/blog-img/git%E6%89%8B%E5%86%8C-2.png)
> 
> 可以直接写到git项目中，与.git同级目录，内容示例如下：
> 
> ![](https://cdn.jsdelivr.net/gh/youxt-njnu/blog-img/git%E6%89%8B%E5%86%8C-3.png)

**从其他服务器克隆一个已存在的git仓库**

git clone 远程地址 将远程仓库的内容克隆到本地 

```
$ git clone https://github.com/atguiguyueyue/git-shTest.git 
Cloning into 'git-shTest'... 
remote: Enumerating objects: 3, done. 
remote: Counting objects: 100% (3/3), done. 
remote: Compressing objects: 100% (2/2), done. 
remote: Total 3 (delta 0), reused 3 (delta 0), pack-reused 0 
Unpacking objects: 100% (3/3), done.
```

**拉取远程分支的最新代码**

git pull # 从远程仓库，把最新的代码下载导本地，保持当前分支代码和远程分支一致

git pull 远程库地址别名 远程分支名 将远程仓库对于分支最新内容拉下来后与当前本地分支直接合并

```
$ git pull ori master 
remote: Enumerating objects: 5, done. 
remote: Counting objects: 100% (5/5), done. 
remote: Compressing objects: 100% (1/1), done. 
remote: Total 3 (delta 1), reused 3 (delta 1), pack-reused 0 
Unpacking objects: 100% (3/3), done. 
From https://github.com/atguiguyueyue/git-shTest 
 * branch            master     -> FETCH_HEAD 
   7cb4d02..5dabe6b  master     -> ori/master 
Updating 7cb4d02..5dabe6b 
Fast-forward 
 hello.txt | 2 +- 
 1 file changed, 1 insertion(+), 1 deletion(-) 
```

**删除远程分支**

```
# 删除远程仓库中，指定名称的远程分支
git push 远程仓库名称 --delete 远程分支名称
$ git push origin --delete pay
# 删除本地分支,如果pay分支还未被合并，-d命令会报错，-D命令不会报错
$ git push origin -d pay
$ git push origin -D pay
```

# 报错处理

[提醒多用户拥有：**fatal: detected dubious ownership in repository at ‘E:/workspace/CSMarket**](https://blog.51cto.com/u_15740728/5805813)

[解决git报错10054](https://blog.csdn.net/weixin_40908748/article/details/122367878)

[破除大文件限制](https://www.jianshu.com/p/7d8003ba2324)

[大文件检测删除](https://marcosantonocito.medium.com/fixing-the-gh001-large-files-detected-you-may-want-to-try-git-large-file-storage-43336b983272)

[Git冲突：Please commit your changes or stash them before you merge_1024_Byte的博客-CSDN博客](https://blog.csdn.net/DDD4V/article/details/118896307)

[Git Pull Force——如何用 Git 覆盖本地更改](https://www.freecodecamp.org/chinese/news/git-pull-force-how-to-overwrite-local-changes-with-git/)

> 1、保留本地的修改
> 
> git stash
> 
> git pull 
> 
> git stash pop
> 
> 2、放弃本地修改
> 
> git reset --hard
> 
> git pull -------- or ------------ git pull ori master(针对报错：There is no tracking information for the current branch. Please specify which branch you want to merge with.可以尝试这个)

# 跨设备迁移

用everything全局搜索了下，以前的我还没给新电脑的git搞个ssh key，于是参考这个[教程](https://www.cnblogs.com/Ye-zixiao/p/12233193.html)的第一步生成了一个；

随后参考这个教程复制旧电脑的.ssh文件，替换到新电脑的.ssh文件：[在不同的电脑上使用同一个git账号_一个git账号可以两个人用吗_你在桥头我在火星的博客-CSDN博客](https://blog.csdn.net/weixin_44227858/article/details/108317689)

还有个这个教程：[自己有两台电脑,如何使用git同步文件 - 知乎 (zhihu.com)](https://zhuanlan.zhihu.com/p/352824096)，留个记录
