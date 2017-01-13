##报错信息1
```
To prevent you from losing history, non-fast-forward updates were rejected Merge the remote changes 
```
###发生原因
git仓库中已经有一部分代码，所以它不允许你直接把你的代码覆盖上去。
###解决方式
1.强推
```
$ git push -f origin master
```
2.先把git的东西fetch到你本地然后merge后再push
```
$ git config branch.master.remote origin
$ git config branch.master.merge refs/heads/master
$ git pull 
$ git push origin master
```



##报错信息2
```
Your branch is ahead of 'origin/master' by 1 commit
```

###发生原因
之前已经有1个commit了

###解决方式
```
git reset --hard HEAD~1
```

####[注意]
执行上面这个命令之后就真的退回去了，需要重新修改，再次提交。














