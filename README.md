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
- 管理分支
每次提交，Git都把它们串成一条时间线，这条时间线就是一个分支。截止到目前，只有一条时间线，在Git里，这个分支叫主分支，即master分支。HEAD严格来说不是指向提交，而是指向master，master才是指向提交的，所以，HEAD指向的就是当前分支。
一开始的时候，master分支是一条线，Git用master指向最新的提交，再用HEAD指向master，就能确定当前分支，以及当前分支的提交点：
![forumImage20160511110244548](http://www.liaoxuefeng.com/files/attachments/0013849087937492135fbf4bbd24dfcbc18349a8a59d36d000/0)

![forumImage20160511110244548](https://ws4.sinaimg.cn/large/006tNc79gy1fh900pnhm4j30sa0a075i.jpg)
每次提交，master分支都会向前移动一步，这样，随着你不断提交，master分支的线也越来越长.

当我们创建新的分支，例如dev时，Git新建了一个指针叫dev，指向master相同的提交，再把HEAD指向dev，就表示当前分支在dev上：
![forumImage20160511110244548](http://www.liaoxuefeng.com/files/attachments/001384908811773187a597e2d844eefb11f5cf5d56135ca000/0)
Git创建一个分支很快，因为除了增加一个dev指针，改改HEAD的指向，工作区的文件都没有任何变化！

不过，从现在开始，对工作区的修改和提交就是针对dev分支了，比如新提交一次后，dev指针往前移动一步，而master指针不变：
![forumImage20160511110244548](http://www.liaoxuefeng.com/files/attachments/0013849088235627813efe7649b4f008900e5365bb72323000/0)
假如我们在dev上的工作完成了，就可以把dev合并到master上。Git怎么合并呢？最简单的方法，就是直接把master指向dev的当前提交，就完成了合并：
![forumImage20160511110244548](http://www.liaoxuefeng.com/files/attachments/00138490883510324231a837e5d4aee844d3e4692ba50f5000/0)
```
$ git checkout -b dev
Switched to a new branch 'dev'
```
git checkout命令加上-b参数表示创建并切换，相当于以下两条命令：
```
$ git branch dev
$ git checkout dev
Switched to branch 'dev'
```
然后，我们就可以在dev分支上正常提交，例如我新增一个文件“test.txt”：
```
➜  GitHubAlipaysdk git:(dev) ✗ git add test.txt
➜  GitHubAlipaysdk git:(dev) ✗ git status
On branch dev
Changes to be committed:
(use "git reset HEAD <file>..." to unstage)

new file:   test.txt

➜  GitHubAlipaysdk git:(dev) ✗ git commit -m 'add test.txt'
[dev 257073a] add test.txt
1 file changed, 0 insertions(+), 0 deletions(-)
create mode 100644 test.txt
➜  GitHubAlipaysdk git:(dev)
```
我们把新增文件合到master分支里，切回master分支
```
➜  GitHubAlipaysdk git:(dev) git checkout master
Switched to branch 'master'
Your branch is up-to-date with 'origin/master'.
➜  GitHubAlipaysdk git:(master) git merge dev
Updating 62c6c28..257073a
Fast-forward
test.txt | Bin 0 -> 16 bytes
1 file changed, 0 insertions(+), 0 deletions(-)
create mode 100644 test.txt
```
git merge命令用于合并指定分支到当前分支。合并后，再查看test.txt的内容，就可以看到，和dev分支的最新提交是完全一样的。
注意到上面的Fast-forward信息，Git告诉我们，这次合并是“快进模式”，也就是直接把master指向dev的当前提交，所以合并速度非常快。
合并完成后，就可以放心地删除dev分支了：
```
➜  GitHubAlipaysdk git:(master) git branch -d dev
Deleted branch dev (was 257073a).
```
- 合并分支
你要知道的第一件事是，git rebase 和git merge 做的事其实是一样的。它们都被设计来将一个分支的更改并入另一个分支，只不过方式有些不同。
###举例说明：我从 dev 拉了3个 feature 分支 ，A 、B、C 分支同时改了一个文修的 随便改点啥吧 比如 ：README.md
```
➜  wytGitHub git:(development) git branch
* development
feature/feature-A
feature/feature-B
feature/feature-C
```
ok ，A 、B、C 分支已经同时修改了一个文件，现在我们切回development 开始合并
```
➜  wytGitHub git:(development) git merge feature/feature-A
Auto-merging README.md
CONFLICT (content): Merge conflict in README.md
Automatic merge failed; fix conflicts and then commit the result.
```
告诉我有冲突了
![forumImage20160511110244548](https://ws2.sinaimg.cn/large/006tNc79gy1fh91jkzrc3j30h4052js5.jpg)
![forumImage20160511110244548](https://ws3.sinaimg.cn/large/006tNc79gy1fh91jmq32bj30uo08atax.jpg)
明确告诉冲突的地方并且是哪个分支，假设我们不featureA的需求合进来把"<<<"，那就把这些删除。然后提交
```
➜  wytGitHub git:(development) ✗ git add README.md
➜  wytGitHub git:(development) ✗ git commit -m 'merge featureA'
[development aa78dc1] merge featureA
```
featureA的需求已经合入 development，下一步合B分支：
```
git merge feature/feature-B
Auto-merging README.md
CONFLICT (content): Merge conflict in README.md
Automatic merge failed; fix conflicts and then commit the result.
```
把冲突解决完后，然后add commit .... C分支也是同样不在描述
刚开始你的分支是这样的
![forumImage20160511110244548](https://camo.githubusercontent.com/919155232a162011b8a64a84872482034c0ce999/68747470733a2f2f7777772e61746c61737369616e2e636f6d2f6769742f696d616765732f7475746f7269616c732f616476616e6365642f6d657267696e672d76732d7265626173696e672f30312e737667)

feature分支中新的合并提交(merge commit)将两个分支的历史连在了一起。你会得到下面这样的分支结构：
![forumImage20160511110244548](https://camo.githubusercontent.com/266aef357af48c218b8779cd489bcb8839aa0a25/68747470733a2f2f7777772e61746c61737369616e2e636f6d2f6769742f696d616765732f7475746f7269616c732f616476616e6365642f6d657267696e672d76732d7265626173696e672f30322e737667)

Merge好在它是一个安全的操作。现有的分支不会被更改，避免了rebase潜在的缺点。

另一方面，这同样意味着每次合并上游更改时feature分支都会引入一个外来的合并提交。如果master非常活跃的话，这或多或少会污染你的分支历史。虽然高级的git log 选项可以减轻这个问题，但对于开发者来说，还是会增加理解项目历史的难度。

比如还用我上面的例子，我们看一下使用一下 git log 查看一下分支的合并情况：
git log --graph --pretty=oneline --abbrev-commit
```
*   b536bf4 merge feature-b
|\
| * 859551c 修改feature-b
| * 6e770a2 commit -a
* |   aa78dc1 merge featureA
|\ \
| |/
|/|
```
###Rebase
作为merge的替代选择，你可以像下面这样将feature分支并入master分支：
```
git rebase development
First, rewinding head to replay your work on top of it...
Applying: 修改feature-c
```
用 git log --oneline 命令查看一下提交历史：
```
b4cbcd9 修改feature-c
b536bf4 merge feature-b
aa78dc1 merge featureA
859551c 修改feature-b
fb1c578 修改A分支readme
a5b4e93 commit understand how stage work
6f5a9fc change md
6e770a2 commit -a
2b5f6e4 修改md文档
```
![forumImage20160511110244548](https://ws3.sinaimg.cn/large/006tNc79gy1fh940zol3tj30c504kdfn.jpg)
rebase最大的好处是你的项目历史会非常整洁。首先，它不像git merge 那样引入不必要的合并提交。其次，如上图所示，rebase导致最后的项目历史呈现出完美的线性——你可以从项目终点到起点浏览而不需要任何的Fork。这让你更容易使用git log 、git bisect 和gitk 来查看项目历史

#####需要注意：rebase的时候，修改冲突后的提交不是使用commit命令，而是执行rebase命令指定 --continue选项。若要取消rebase，指定 --abort选项，完成后进行 git push -f 操作

- 合并commit历史
```
f46a946 change2
86f8f56 change 1
b4cbcd9 修改feature-c
b536bf4 merge feature-b
```
假设我有四个提交记录 ， 我想把前三个合并一个提交，可以这样操作：
```
git rebase -i b536bf4
```
按回车进入下一个页面：
```
pick b4cbcd9 修改feature-c
pick 86f8f56 change 1
pick f46a946 change2
Rebase b536bf4..f46a946 onto b536bf4 (3 command(s))
Commands:
p, pick = use commit
r, reword = use commit, but edit the commit message
e, edit = use commit, but stop for amending
s, squash = use commit, but meld into previous commit
f, fixup = like "squash", but discard this commit's log message
x, exec = run command (the rest of the line) using shell
d, drop = remove commit

These lines can be re-ordered; they are executed from top to bottom.

If you remove a line here THAT COMMIT WILL BE LOST.

However, if you remove everything, the rebase will be aborted.

Note that empty commits are commented out
```
前面三行是我们需要操作的三个 Commit，每行最前面的是对该 Commit 操作的 Command。关于每个 Command 具体做什么，下面的注释写得非常清楚。为了完成我们的需求，我们可以关注到这两个命令：
```
s, squash = use commit, but meld into previous commit
f, fixup = like "squash", but discard this commit's log message
```
- squash：使用该 Commit，但会被合并到前一个 Commit 当中
- fixup：就像 squash 那样，但会抛弃这个 Commit 的 Commit message

看样子两个命令都可以完成我们的需求，那么让我们先试一下 squash！由于我们是想把三个 Commit 都合并在一起，并且使 Commit Message 写成 Commit-1，所以我们需要把 86f8f56(Commit-2) 和 f46a946  (Commit-3) 前面的 pick 都改为squash，于是它看起来像这样：
```
pick b4cbcd9 修改feature-c
s 86f8f56 change 1
s f46a946 change2
```
完成后，使用 :wq 保存并退出。这个时候，我们进入到了下一个界面：
```
This is a combination of 3 commits.
The first commit's message is:
修改feature-c

This is the 2nd commit message:

change 1

This is the 3rd commit message:

change2

Please enter the commit message for your changes. Lines starting
with '#' will be ignored, and an empty message aborts the commit.

Date:      Wed Jul 5 15:24:18 2017 +0800

interactive rebase in progress; onto b536bf4
Last commands done (3 commands done):
s 86f8f56 commit-2
s f46a946 commit-3
No commands remaining.
```
通过下面的注释，我们可以知道，这里其实就是一个编写 Commit Message 的界面，带 # 的行会被忽略掉，其余的行就会作为我们的新 Commit Message。为了完成我们的需求，我们修改成这样：
```
This is a combination of 3 commits.
The first commit's message is:
修改feature-c


Please enter the commit message for your changes. Lines starting
with '#' will be ignored, and an empty message aborts the commit.

Date:      Wed Jul 5 15:24:18 2017 +0800

interactive rebase in progress; onto b536bf4
Last commands done (3 commands done):
s 86f8f56 commit-2
s f46a946 commit-3
No commands remaining.
```
保存 git log --online 
```
c570efc 修改feature-c
b536bf4 merge feature-b
```
任务完成。

- 修改提交历史
有的时候觉得我们commit 信息不够清晰，我们就需手动修改提交历史，比如我想修改 b536bf4 这个历史
```
c570efc 修改feature-c
b536bf4 merge feature-b
```
可以这样做:
```


➜  wytGitHub git:(feature/feature-c) git rebase -i b536bf4
e c570efc 修改feature-c

Rebase b536bf4..c570efc onto b536bf4 (1 command(s))

Commands:
p, pick = use commit
r, reword = use commit, but edit the commit message
e, edit = use commit, but stop for amending
s, squash = use commit, but meld into previous commit
f, fixup = like "squash", but discard this commit's log message
x, exec = run command (the rest of the line) using shell
d, drop = remove commit

保存wq 


Stopped at c570efcf6f9c0a49f35192549d60d7e4285da11b... change feature-c:
You can amend the commit now, with

git commit --amend

Once you are satisfied with your changes, run

git rebase --continue
```
这些指示很明确地告诉了你该干什么。输入: git commit --amend
```
修改feature-c 修改为"change test"

# Please enter the commit message for your changes. Lines starting
# with '#' will be ignored, and an empty message aborts the commit.
```
保存退出

```
git rebase --continue
Successfully rebased and updated refs/heads/feature/feature-c.
```
查看历史 415bbc7 change test 修改成功

###问题：merge  和 rebase 使用场景
两个使用场景是不一样的，merge只是合并另外一个分支的内容，rebase也合并另外一个分支的内容，但是会把本分支的commits顶到最顶端
假设我们现在有3个分支

master分支：线上环境使用的分支
testing分支：测试环境使用的分支
my_feature分支：开发新功能的分支，也就是当前分支

A. 假设我在my_feature上开发了一段时间，之后另外的同事开发的功能正式上线到master分支了，那么我可以在当前的分支下rebase一下master分支，这样我这个分支的几个commits相对于master还是处于最顶端的，也就是说rebase主要用来跟上游同步，同时把自己的修改顶到最上面
B. 我在my_feature上开发了一段时间了，想要放到testing分支上，那就切到testing，然后merge my_feature进来，因为是个测试分支，commits的顺序无所谓，也就没必要用rebase (当然你也可以用rebase)

另外，单独使用rebase，还有调整当前分支上commits的功能(合并，丢弃，修改commites msg)
1. 用merge确实只需要解决一遍冲突，比较简单粗暴
2. 用rebase有时候会需要多次fix冲突（原因在于本地分支已经提交了非常多的commit，而且很久都没有和上游合并过）我个人推荐大家开发的时候，尽量及时rebase上游分支，有冲突提前就fix掉，即使我们自己的分支开发了很久（哪怕是几个月），也不会积累太多的conflict，最后合并进主分支的时候特别轻松， 非常反对从master check出新分支，自己闷头开发几个月，结果最后merge进主分支的时候，一大堆冲突，自己还嗷嗷叫的行为
3.对于两个分支而言，rebase和merge没有区别，但是rebase更干净，因为log hisitory是线性的，但commit不一定按日期先后排，而是local commit总在后面,merge之后history变得比较复杂，但是commit按日期排序,stackoverflow上有个图示很好:

































