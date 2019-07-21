## GIT 实践操作记录 偏实用 

####  基本概念

​    [最好的git简易指南](http://rogerdudler.github.io/git-guide/index.zh.html)

1. 工作区，暂存区和仓库  work->stage->repository(.git文件)
2. HEAD指向某个commit的指针，由于是指针操作，git在合并分支很迅速。

#### 开始准备

* github上新建一个空的远端仓库 最好不要勾选添加readme.md，不然直接推送本地已有仓库会比较麻烦。

* git init 初始化本地仓库   

  git remote add origin 你的远程库地址  // 把本地库与远程库关联    

  git push -u origin master    // 第一次推送时

*  git config --global [alias.st](http://alias.st/) status 给命令配置简写 全局生效

#### 工作区（项目文件夹，就是init git的命令所在的当前整个目录）

* git st看到的红色的就是工作区修改的文件


* git add file 添加工作区该文件到暂存区
* git add . 添加工作区当前目录所有文件到暂存区

#### 暂存区（git st看到的文件颜色是绿色的）

* git commit -m "comment"

* git commit --amend  修改提交的comment   

* git commit -a --amend 追加修改到上次的提交信息（上次的提交不能push，否则pull会冲突，需要rebase；如果确认远端没有被别人更新或者拉取 push -f）


  * [git rebase](http://jartto.wang/2018/12/11/git-rebase/) 


* git 撤销提交到指定commitid
   *    git reset --hard fabab51e
* git回退commit但是没有push的版本
   * git reset -hard id  
     完成撤销，同时将代码恢复到前一个commit_id对应的版本
     **还可以回退pull后的冲突到pull前的状态** 
   * git reset id （可以做到回退到提交之前的状态，不影响工作区已经做出的更改。可以直接通过git commit 重新提交对本地代码的修改 🔥 reset恢复暂存区的文件到工作区 reset--hard重置暂存区和工作区，使之和上次提交保持一致 。和git checkout .区别：git checkout将工作区内容撤回，工作区做出的修改是红色的，暂存区是绿色的） 
* git让远端分支完全覆盖本地分支
   * git fetch --all   

   * git reset --hard origin/master  

   *  git pull


#### 分支操作


* git 分支命令
   * git checkout dev（~~如果没有dev 则新建一个分支~~切换到dev）
   * :smile: git checkout  --orphan guer 创建没有父节点的孤儿分支，但是文件还在，可以git rm -rf .移除孤儿分支下文件。此时git branch -a看不到，要发生commit就可以看到。
   * git checkout -b dev（基于当前分支克隆一个分支并切换到dev分支 等同于git branch   dev +git checkout dev）
   * git branch -a 显示本地和远端的分支
   * git branch -D dev（强制删除dev分支）
   * git branch -d dev（删除已经完全merge到上游的分支，或者head没有trace/没有set upstream
   * git branch --list(查看分支列表)
   
* git pull如果有冲突 可以回退到提交前的状态 然后把本地工作区的内容stash，pull后pop出，如果只想apply某次stash，要git stash apply id
    * git commit -a --amend 但是要注意 只能是本地的已经提交没有push的改变
    * git stash drop stash{0}丢弃某一个stash，git stash clear清楚所有的stash
* git对于修改了没有add到暂存区的文件，可以用checkout放弃对工作区修改
* git push -f强制覆盖远程分支
*  git reset head 恢复提交到暂存区的修改，重新回到工作区
   * git reset --hard versionnumber
  * git reset --hard HEAD^ (上一个版本就是HEAD^，上上一个版本就是HEAD^^)
  * git checkout 是撤销本地暂存区的修改，使工作区clean
  * git clean -df 强制清除未被追踪的文件
  * git config --global alias.st status 给命令配置简写 全局生效
  * git 从暂存区的提交 撤回到工作区状态  git reset id（上次提交的idhash值）
  * 如果当前很久没有pull commit后pull，冲突，git rebase --continue 一步步解决冲突的commit，忽略用git add . 继续即可。git的提示非常只能，根据提示操作即可
  * git remote 
    * git remote rm origin删除本地关联的远端的分支
    * git remote -v 查看本地关联的远端分支
    * git remote add origin (your remote address)

#### 高级操作
  * git reflog 查看过去操作日志，记录了历史记录 继续回到回退版本
  * git cherry-pick commitId 将某个分支的一个commit拉取到当前分支
  * git rebase -i id git合并提交信息（此命令会进入交互式命令行，然后会有提示p表示选中，s表示合并到上一次提交信息上，e表示修改档次的提条信息）see:https://www.jianshu.com/p/4a8f4af4e803
* git rebase branch 合并新的commit到当前分支，但是不能直接pull pull要加rebase参数 否则冲突
* git pull --rebase 远端分支覆盖本地分支 本地修改放弃
* git reflog show chenzg 查看分支的父节点，即是从那个分支检出的。
* git branch -vv可以看本地分支追踪的远端分支
* git branch dev commitid 从当前分支新建一个dev分支，指向指定commit；**❀重点 分支其实也是指向某个`commit`的指针，`HEAD`也是一个指针，它指向当前工作目录下的`commit`**  
   * git branch --set-upsteam dev origin/dev将本地dev和远端建立追踪关系
   * git push origin chenzg:chenzg 本地命令添加远端分支
   * git push origin --delete chenzg 本地命令删除远端分支
   * git remote prune origin 可以删除缓存在本地的远端已经删除的分支

