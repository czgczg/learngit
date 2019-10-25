### 基本操作

site conference

* git command --help 帮助文档

- [最好的git指南](http://rogerdudler.github.io/git-guide/index.zh.html)
- [git rebase](http://jartto.wang/2018/12/11/git-rebase/)
- [git  scm](https://git-scm.com/book/zh/v2) 

基本概念： 工作区（.git所在的目录 修改的文件是红色的） 暂存区（add . 到暂存区 文件是绿色的） 仓库（commit到此）

添加到暂存区 `git add .`

提交到本地仓库`git commit -m 'message comment'`

更新远程仓库到本地`git pull`,如果没有是第一次，要关联到远端分支（set--upsteam-to branch dev）

推动本地仓库到远端分支`git push`,如果是第一次要`git push -u origin master`

git fetch --all后`Your branch is ahead of 'origin/test' by 1 commit.` 确保可以直接push

`Your branch and 'origin/test' have diverged,and have 1 and 2 different commits each, respectively` 这种时候 远端分支的更新会merge 插入到本地commit记录中，默认会产生一条merge的提交记录

### 常用

- git config 给命令配置简写 全局生效
  - `git config --global alias.st status`
  - `git config --global alias.checkout co`

 .......

- git 撤销操作
  - git reset --hard fabab51e（放弃本地所有修改）
  - git reset id 可以（可以撤销提交，本地修改依然在，可以继续添加提交）
  - git reset head^ （撤销最新一次提交）
  - git checkout . （放弃本地工作区的修改）
  - git reset . （撤销添加到暂存区文件，回到工作区）
- git clean
  - git clean -n 显示将要删除的文件和目录
  - git clean -x 删除忽略文件和对git来说不识别的文件
  - git clean -d 删除未被添加到git的路径中的文件
  - git clean -df 强制删除所有未追踪的和识别的文件
- git分支
  - git checkout dev（如果没有dev 则新建一个分支）
  - git branch -D dev（删除dev分支）
  - git branch --list(查看分支列表)
  - git remote prune origin 可以删除缓存的远端已经删除的分支
  - *git checkout --orphan empty* 新建空的分支 然后移除里面文件成为真正的空分支
- git remote
  - git remote rm origin删除本地关联的远端的分支
  - git remote -v 查看本地关联的远端分支
  - git remote add origin (your remote address)

### **冲突** 😭 😭 😭

 🔥 一般操作 比对 备份 本地回滚

👉没有图形工具找冲突的文件git diff --name-only --diff-filter=U

👉 让远端分支完全覆盖本地 `1.git fetch --all 2.git reset --hard origin/master 3.git pull`

 stash压栈操作命令

 `git stash`

 `git stash list` 看stash列表

 `git stash pop`推荐

 `git stash apply id`

 `git stash clear`清楚所有的stash

 `git stash show -p stash@{1}`看指定stash内容

 `git stash drop stash{0}`丢弃某一个stash

cherry-pick剪出某提交命令

 `git cherry-pick commitId`将commit迁移到当前分支

😄 规范操作 防范未然

 两种情况：

 第一：写好了功能要提交代码

1. git commit -a -m 'msg'

2. git fetch --all

3. **git status** 看！！ 会提示你本地分支的远端分支的比对情况 update/ahead/behind/diverged？

4. 如果分歧或者ahead/behind太多，切到tmp临时分支保留提交

5. 切回主分支，git reset --hard lastCommitId。切到上次update的提交，保证和远端没有冲突，然后pull拉去最新更新，然后将tmp临时分支上的指定commit cherry-pick过来，即便有冲突，会提示冰自动合并，解决>>>>冲突后，再提交一次即可。

   第二：仅仅是更新代码，本地不想提交

1. git stash压栈
2. git status 看到是新的干净状态
3. git pull
4. git stash pop
5. 有冲突，会自动合并，解决完后，手动清理stash



## 高级

- rebase变基 改变时间线 做到干净的一条直线提交记录，但是要保证在不冲突情况下使用，冲突请用merge
  - git rebase branch 合并新的commit到当前分支
  - git rebase -i id git合并提交信息（此命令会进入交互式命令行，然后会有提示p表示选中，s表示合并到上一次提交信息上，e表示修改档次的提条信息）see:[参考](https://www.jianshu.com/p/4a8f4af4e803)
  - 如果当前很久没有pull commit后pull，冲突，git rebase --continue 一步步解决冲突的commit，忽略用git add . 继续即可 git的提示非常只能，根据提示操作即可
  - **git reflog** 查看过去操作日志，记录了历史记录操作 可以回到回退任意版本

### 不常用但有用的

- git commit --amend 修改提交的comment message
- git commit -a --amend 追加修改到上次相同的提交comment（前提是不能push）否则会一堆冲突 rebase解决合并问题，如果想该已经push的commit信息，可以push -f
- git ls-files -t 显示被git追踪的文件
- git branch -vv 看本地分支和远端分支的关联，不过一般名字都一样，没太大用
- git blame file看文件被是谁改的，加参数哪一行谁提交的
- **git show head** 看工作区和本地仓库的文件改动
- git diff --staged 显示暂存区和上一条提交的不同
- **git diff** 显示工作区和暂存区的不同
- git diff head 显示工作区和上一条提交的不同
- git diff commitid commitid 显示commit的改动不同

## END

git 自带的gitk可以看提交记录，git gui可以提交和看暂存区文件。但是功能局限。

git smart和sourcetree试用了下，体验不大好

建议直接用git gortoise小乌龟进行pull操作，图形界面可以方便的看到冲突文件，便捷的接受谁的部分，放弃自己部分。不用每一行都要去检查到底用谁的。（特别是些配置文件，大家频繁的改，接受谁的都差不多）。

别的 操作，命令行足够，快而且提示信息多。