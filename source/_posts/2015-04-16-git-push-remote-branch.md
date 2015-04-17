title: "git操作 - push本地分支到远端分支"
date: 2015-04-16 01:57:25
---

##1.背景：
一般情况下我们的git的操作会是这样的

``` bash
$ git clone git@xxx.git local_repo
$ ... do some changes in local repo
$ git push origin master
```

最后的push操作会将本地的master分支更新到远端origin的master分支上去(由于git clone时默认设置了本地master分支的up-stream为origin/master)。

那么问题来了

* *如何将本地分支推送到远端的别的分支,比如origin/other_branch,即使在远端这个分支不存在*

##2.解决方案
	$ git push origin master:other_branch

这样之后，本地的master分支就提交到了远端的other_branch,同时该master分支的up-stream也被设置到了该分支。但是以后每次更新的时候是否还需要再次打这么长一条命令，能否直接git push 就可以呢?
首先根据[\[2\]][2],我们可以知道本地分支的track信息:

`$ git remote show origin`
  
发现master分支push的时候仍会push到remote/origin的master分支。根据[\[1\]][1],我们可以知道git push时如果不加任何东西，会按照push.default的这个默认配置来push。而这个push.default定义如下:

	push.default
	Defines the action git push should take if no refspec is given on the
	command line, no refspec is configured in the remote, and no refspec 
	is implied by any of the options given on the command line. 
	
	Possible values are:
	nothing - do not push anything.
	matching - push all matching branches. All branches having the same 
	name in both ends are considered to be matching. This is the default.
	upstream - push the current branch to its upstream branch.
	tracking - deprecated synonym for upstream.
	current - push the current branch to a branch of the same name.

所以我们要做的就是把push的策略改为upstream:

`$ git config --config push.default=upstream`

现在再使用git push,此时本地的master分支就会push到remote的hexosrc分支上了(但是我使用用`git remote show origin`似乎没有变化,不知是否是bug)。
##3.其他
删除远端分支

	$ git push origin :other_branch  #:右边的远端分支other_branch将被删除。

初看好像挺难理解这条命令的逻辑的，如果把这条命令想成把一个空分支提交到远端other_branch分支，那么这条命令的语义似乎也好理解了。

---
####参考文献

[1. http://stackoverflow.com/questions/8170558/git-push-set-target-for-branch][1]  
[2. http://stackoverflow.com/questions/171550/find-out-which-remote-branch-a-local-branch-is-tracking][2]

[1]: http://stackoverflow.com/questions/8170558/git-push-set-target-for-branch
[2]: http://stackoverflow.com/questions/171550/find-out-which-remote-branch-a-local-branch-is-tracking