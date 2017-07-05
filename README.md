#Git 简介：

Git是一个开源的分布式版本控制系统，用于敏捷高效地处理任何或小或大的项目。
Git 是 Linus Torvalds 为了帮助管理 Linux 内核开发而开发的一个开放源码的版本控制软件。
Git 与常用的版本控制工具 CVS, Subversion 等不同，它采用了分布式版本库的方式，不必服务器端软件支持。

###Git与SVN的区别。
先说集中式版本控制系统，版本库是集中存放在中央服务器的，而干活的时候，用的都是自己的电脑，所以要先从中央服务器取得最新的版本，然后开始干活，干完活了，再把自己的活推送给中央服务器。中央服务器就好比是一个图书馆，你要改一本书，必须先从图书馆借出来，然后回到家自己改，改完了，再放回图书馆。
![forumImage20160511110244548](http://ww2.sinaimg.cn/large/006tNc79gy1fh6w82gmmaj30jg0ey0vk.jpg)
集中式版本控制系统最大的毛病就是必须联网才能工作，如果在局域网内还好，带宽够大，速度够快，可如果在互联网上，遇到网速慢的话，可能提交一个10M的文件就需要5分钟。


那分布式版本控制系统与集中式版本控制系统有何不同呢？首先，分布式版本控制系统根本没有“中央服务器”，每个人的电脑上都是一个完整的版本库，这样，你工作的时候，就不需要联网了，因为版本库就在你自己的电脑上。既然每个人电脑上都有一个完整的版本库，那多个人如何协作呢？比方说你在自己电脑上改了文件A，你的同事也在他的电脑上改了文件A，这时，你们俩之间只需把各自的修改推送给对方，就可以互相看到对方的修改了。

和集中式版本控制系统相比，分布式版本控制系统的安全性要高很多，因为每个人电脑里都有完整的版本库，某一个人的电脑坏掉了不要紧，随便从其他人那里复制一个就可以了。而集中式版本控制系统的中央服务器要是出了问题，所有人都没法干活了。

在实际使用分布式版本控制系统的时候，其实很少在两人之间的电脑上推送版本库的修改，因为可能你们俩不在一个局域网内，两台电脑互相访问不了，也可能今天你的同事病了，他的电脑压根没有开机。因此，分布式版本控制系统通常也有一台充当“中央服务器”的电脑，但这个服务器的作用仅仅是用来方便“交换”大家的修改，没有它大家也一样干活，只是交换修改不方便而已。
![forumImage20160511110244548](http://ww3.sinaimg.cn/large/006tNc79gy1fh6w43son4j30bf0890sp.jpg)

###工作区和暂存区
Git和其他版本控制系统如SVN的一个不同之处就是有暂存区的概念。
- 什么工作区？
工作区就是你电脑能看到的目录，比如我们的工程 XXX 文件夹

- 版本库（Repository）
工作区有一个隐藏目录.git，这个不算工作区，而是Git的版本库。
Git的版本库里存了很多东西，其中最重要的就是称为stage（或者叫index）的暂存区，还有Git为我们自动创建的第一个分支master，以及指向master的一个指针叫HEAD。

![forumImage20160511110244548](http://www.liaoxuefeng.com/files/attachments/001384907702917346729e9afbf4127b6dfbae9207af016000/0)

第一步是用git add把文件添加进去，实际上就是把文件修改添加到暂存区；

第二步是用git commit提交更改，实际上就是把暂存区的所有内容提交到当前分支。

因为我们创建Git版本库时，Git自动为我们创建了唯一一个master分支，所以，现在，git commit就是往master分支上提交更改。

你可以简单理解为，需要提交的文件修改通通放到暂存区，然后，一次性提交暂存区的所有修改。

```
➜  wytGitHub git:(feature/feature-A) ✗ git status
On branch feature/feature-A
Your branch is up-to-date with 'origin/feature/feature-A'.
Changes not staged for commit:
(use "git add <file>..." to update what will be committed)
(use "git checkout -- <file>..." to discard changes in working directory)

modified:   README.md

Untracked files:
(use "git add <file>..." to include in what will be committed)

test.txt

no changes added to commit (use "git add" and/or "git commit -a")
```
我修改改了 README.md 这个文档，并且往工作区新增了一个test.txt的文件，Git非常清楚地告诉我们，README.md被修改了，而test还从来没有被添加过，所以它的状态是Untracked。

现在，使用两次命令git add，把readme.txt和LICENSE都添加后，用git status再查看一下：
```
➜  wytGitHub git:(feature/feature-A) ✗ git add 'test.txt'
➜  wytGitHub git:(feature/feature-A) ✗ git add 'README.md'
➜  wytGitHub git:(feature/feature-A) ✗ git status
On branch feature/feature-A
Your branch is up-to-date with 'origin/feature/feature-A'.
Changes to be committed:
(use "git reset HEAD <file>..." to unstage)

modified:   README.md
new file:   test.txt

➜  wytGitHub git:(feature/feature-A) ✗
```
现在我已经把两个文件加入到暂存区了
![forumImage20160511110244548](http://www.liaoxuefeng.com/files/attachments/001384907720458e56751df1c474485b697575073c40ae9000/0)

git add命令实际上就是把要提交的所有修改放到暂存区（Stage），然后，执行git commit就可以一次性把暂存区的所有修改提交到分支。
```
➜  wytGitHub git:(feature/feature-A) ✗ git commit -m 'commit understand how stage work'
[feature/feature-A a5b4e93] commit understand how stage work
2 files changed, 1 insertion(+), 1 deletion(-)
create mode 100644 test.txt
➜  wytGitHub git:(feature/feature-A) git status
On branch feature/feature-A
Your branch is ahead of 'origin/feature/feature-A' by 1 commit.
(use "git push" to publish your local commits)
nothing to commit, working directory clean
```
一旦提交后，如果你又没有对工作区做任何修改，那么工作区就是“干净”的
![forumImage20160511110244548](http://www.liaoxuefeng.com/files/attachments/0013849077337835a877df2d26742b88dd7f56a6ace3ecf000/0)

###Git常用命令
-  版本回滚
![forumImage20160511110244548](http://ww4.sinaimg.cn/large/006tNc79gy1fh6x9cd55vj31kw0ytao8.jpg)
如上图：
例如我想回滚到上一个版本 可以使用 git reset 命令
```
➜  wytGitHub git:(feature/feature-A) git reset --hard HEAD^
HEAD is now at 2b5f6e4 修改md文档
```
指定回退到某一版本：
##### - -oneline 标记把每一个提交压缩到了一行中。它默认只显示提交ID和提交信息的第一行。git log --oneline的输出一般是这样的：
```
2b5f6e4 修改md文档
79744bd change file
9345438 change md
1d085b2 change MD
69bf290 change read.me
f97c4cd commit
2da5726 change file
023c107 add podSpec
37f8a9f add file
8a321b6 Initial commit
```
假设要回退到 “2da5726”这个版本，可以使用命令：
```
➜  wytGitHub git:(feature/feature-A) git reset --hard 2da5726
HEAD is now at 2da5726 change file
```
指针已经指向 2da5726 提交，已经回滚到这个版本
####如果需要做修改，提交之后需要处女执行 git push -f 强制覆盖
- HEAD指向的版本就是当前版本，因此，Git允许我们在版本的历史之间穿梭，使用命令git reset --hard commit_id
- 穿梭前，用git log可以查看提交历史，以便确定要回退到哪个版本。
####如果需要做修改，提交之后需要执行 git push -f 强制覆盖，或者拷贝你需要的一些文件，加把head指向最新的commit

- 撤销修改
在日常开发中免不了撤销修改一些东西，例如产品修改某些feature...
例如：可以使用git diff 来查看修改了哪些地方
```
diff --git a/README.md b/README.md
index 47365ca..edd7938 100644
--- a/README.md
+++ b/README.md
@@ -1 +1 @@
-> ## 这。。。。。
+> ## this is a md
```
在这个时候你可以有两种解决方法：
1.你可以删掉最后一行，手动把文件恢复到上一个版本的状态。
2.使用 git checkout -- file 可以丢弃工作区的修改。
```
➜  wytGitHub git:(feature/feature-A) ✗ git checkout -- README.md
➜  wytGitHub git:(feature/feature-A) git status
On branch feature/feature-A
Your branch is up-to-date with 'origin/feature/feature-A'.
nothing to commit, working directory clean
```


