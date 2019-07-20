### GIT
* site
  * [最好的git简易指南](http://rogerdudler.github.io/git-guide/index.zh.html)
  * [git rebase](http://jartto.wang/2018/12/11/git-rebase/) 
* git 修改提交的comment
    *  git commit --amend
* git 撤销提交到指定commitid
   *    git reset --hard fabab51e

* git让远端分支完全覆盖本地分支
   * git fetch --all   
   * git reset --hard origin/master  
   *  git pull


* git 追加修改到上次相同的提交comment
   * git cm -a --amend（前提不能push）否则会一堆冲突 rebase解决合并问题，如果想该已经push的commit信息，可以push -f
* git 切换分支
   * git checkout dev（如果没有dev 则新建一个分支）
   * git branch -a 显示本地和远端的分支
   * git branch -D dev（删除dev分支）
   * git branch --list(查看分支列表)
   * git remote prune origin 可以删除缓存的远端已经删除的分支
* git pull如果有冲突 可以回退到提交前的状态 然后把本地工作区的内容stash，pull后pop出，如果只想apply某次stash，要git stash apply id
    * git commit -a --amend 但是要注意 只能是本地的已经提交没有push的改变
    * git stash drop stash{0}丢弃某一个stash，git stash clear清楚所有的stash
* git对于修改了没有add到暂存区的文件，可以用checkout放弃对工作区修改
* git回退commit但是没有push的版本
  * git reset -hard id  
    完成撤销，同时将代码恢复到前一个commit_id对应的版本
    **还可以回退pull后的冲突到pull前的状态** 
  *  git reset id （可以做到回退到提交之前的状态，不影响工作区已经做出的更改。可以直接通过git commit 重新提交对本地代码的修改）

* git push -f强制覆盖远程分支
* git cherry-pick commitId 将某个分支的一个commit拉取到当前分支
* git pull --rebase 远端分支覆盖本地分支 本地修改放弃
* git rebase -i id git合并提交信息（此命令会进入交互式命令行，然后会有提示p表示选中，s表示合并到上一次提交信息上，e表示修改档次的提条信息）see:https://www.jianshu.com/p/4a8f4af4e803
*  git reset head 恢复提交到暂存区的修改，重新回到工作区
   * git reset --hard versionnumber
  * git reset --hard HEAD^ (上一个版本就是HEAD^，上上一个版本就是HEAD^^)
  * git checkout 是撤销本地暂存区的修改，使工作区clean
  *  git config --global alias.st status 给命令配置简写 全局生效
  * git reflog 查看过去操作日志，记录了历史记录 继续回到回退版本
  * git 从暂存区的提交 撤回到工作区状态  git reset id（上次提交的idhash值）
  * 如果当前很久没有pull commit后pull，冲突，git rebase --continue 一步步解决冲突的commit，忽略用git add . 继续即可。git的提示非常只能，根据提示操作即可
  * git remote 
    * git remote rm origin删除本地关联的远端的分支
    * git remote -v 查看本地关联的远端分支
    * git remote add origin (your remote address)