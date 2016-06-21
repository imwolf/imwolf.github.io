title: Git使用
date: 2016-06-18 14:18:21
tags: git
---
### git pull
1. 含义:取回远程主机某个分支的更新，再与本地的指定分支合并

```bash
	$ git pull <远程主机名> <远程分支名>:<本地分支名> 
	$ git pull --rebase 以rebase方式进行合并 
	$ git pull = git fecth + git merge 
```

### git diff
```bash
	$ git diff #列出没有进行索引(staged)的修改
	$ git diff --staged(cached) #列出索引内容(通过git add)和上次提交(commit)的区别
```
* 也就是说`git diff`比较工作目录和暂存(索引)区的内容;`git diff --staged`比较暂存区域和历史区域的区别内容  
* git difftool --tool-help可以使用系统上的其他diff工具来进行更加方便得进行内容比较，包括vimdiff,xcode的比较工具等等

### git rm
`git rm`删除文件同时从暂存区中将文件删除  
`git rm --cached`仅仅从暂存区中将文件删除，保留工作目录中的文件

### git log
git log用来显示提交(commit)的历史，如:
```bash
	$ git log -2 #显示前两条提交历史
	$ git log --prety=oneline #单行显示提交历史
	$ git log --since=2.weeks
	$ git log -Sfunction_name
```
	
选项		|描述     
----------|-----------
-p			|显示每次commit时引入的patch
--stat		|显示每次commit时的文件统计信息(修改了几行等)
--prety|用另一种格式显示提交，包括oneline,short,full,fuller,format(自定义)格式
--graph	|使用ASCII图的形式显示分支和合并历史
--relative-date	|显示修改相对日期

其他选项包括--name-only,--name-status,--abbrev-commit
`git log`还包括一些过滤参数，详细参考命令文档或者《pro git》第二章


### 重命名文件
`git`提供了`mv`命令来将文件A重命名为文件B  
`git mv A B`等同于以下三条语句:

```bash
	$ mv A B
	$ git rm A
	$ git add B
```
注意第二句git rm A是很重要的，如果仅仅做了mv操作，然后进行add或者commit -a,就会发现原来的文件并没有被删除掉。
	
### git回退
1. `git reset —hard commit_id`或者 使用HEAD标记,HEAD表示当前版本号，HEAD^上一个版本号，HEAD~100前100个版本号
2. `git log` 查看提交历史，方便回到某个版本;git log --pretty=oneline
3. `git reflog` 查看reflog中的历史，好像是查看reference的变动历史；具体查看help文档；在reset之后可以查到未来的commit_id 

### 工作区与暂存区
1. `git add`的操作都是放在暂存区（不仅仅是添加文件）
2. `git commit`负责把暂存区内的内容提交到版本库

### 撤销修改
1. 修改只在工作区 (before add): `git checkout -- file` (包括误删了文件也可以通过该命令恢复)
2. 修改存在暂存区 (before commit ,after add): `git reset HEAD file` (to unstage the file)
3. 修改已经提交到本地版本库 (after commit): 参见git回退一节
4. **如果已经push到远程版本库呢?**


### 修改commit
如果在一次commit之后发现某个修改没有stage（通过git add)，或者觉得commit message有问题，可以通过`git commit --amend`进行修复，这个命令获取暂存区并将其用于提交

```bash
	$ git commit -m 'initial commit'
	$ git add forgotten_file
	$ git commit --amend
```
上述三条语句最终只产生一个commit


### 将一个Commit分为多个(split commit into 2)
1. 使用`git rebase -i HEAD~3`打开rebase的交互模式，在想要分离的commit那一行选择edit，确定后进入无分支模式HEAD指向此次提交
2. 使用`git reset HEAD~`撤销该commit
3. `git add`需要的修改，commit;再add,再修改
4. `git stash`某些不想提交的文件(否则git rebase --continue无法进行下去)
5. `git rebase --continue`直到完成
6. `git stash list/pop` 得到暂存的文件

>p.s :  
>1) `git rebase --abort `可以中断rebase过程  
>2) `rebase`还可以重调commit顺序，合并commit等等  
>3) 参考[StackOverflow][1] 

超简略版本:

	git rebase -i HEAD~3 -> edit the commit -> git --continue
	
### git remote
	$ git remote -v 
	$ git remote show #可以显示本地分支推送到哪个远程分支

###  新建本地分支与远地分支的联系 
	git branch --set-upstream debug origin/debug

###  [git 重命名分支(本地和远程)][2]
	git branch -m old_branch new_branch
	git push origin :old_branch
	git push --set-upstream origin new_branch

### 分支操作
	git branch --merged #查看已经merge到当前分支的分支(删除这些分支是安全的，他们的工作已经包含在当前分支)
	git branch --no-merged

### 别名操作
	$ git config --global alias.co checkout
	$ git config --global alias.br branch
	$ git config --global alias.ci commit
	$ git config --global alias.st status
	$ git config --global alias.unstage 'reset HEAD --'
	$ git config --global alias.last 'log -1 HEAD'

---
##### 参考文献:
\[1\]:[http://stackoverflow.com/questions/6217156/break-a-previous-commit-into-multiple-commits][1]  
\[2\]:[https://gist.github.com/lttlrck/9628955][2]

[1]:http://stackoverflow.com/questions/6217156/break-a-previous-commit-into-multiple-commits "stackoverflow"
[2]:https://gist.github.com/lttlrck/9628955


