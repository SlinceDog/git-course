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
####如果需要做修改，提交之后需要处女执行 git push -f 强制覆盖

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


