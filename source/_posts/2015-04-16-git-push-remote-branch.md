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

这样之后，本地的master分支就提交到了远端的other_branch,同时该master分支的up-stream也被设置到了该分支，以后pull,push的时候都将会从该分支抓取信息。

##3.其他
删除远端分支

	$ git push origin :other_branch  #:右边的远端分支other_branch将被删除。

初看好像挺难理解这条命令的逻辑的，如果把这条命令想成把一个空分支提交到远端other_branch分支，那么这条命令的语义似乎也好理解了。