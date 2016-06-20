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


