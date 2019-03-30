#### 生成新仓库

`$ git init`

#### 添加文件到git库中

`$ git add readme.txt`

#### 保存所有的修改（修改，删除，添加等）
`$ git add -A`		rather than   `$ git add .`

#### 把文件提交给仓库

`$ git commit -m "wrote a readme file"`

#### clone远程库

`$ git clone git@github.com:JeyserYang/learngit.git`

#### 抓取分支(pull),当我们从远程库clone时，默认只克隆了master分支，如果想要克隆远程库中的其他分支，比如dev分支，用以下命令

`$ git branch -r`		or 		`$ git branch -a`
`$ git checkout -b <branch_name>`

#### 当你push的内容和别人push的内容有冲突时，先用git pull把最新的提交从origin/dev抓下来，然后，在本地合并，解决冲突，再推送。在此之前，需要指定本地dev分支与远程origin/dev分支的链接，根据提示，设置dev和origin/dev的链接
```
$ git branch --set-upstream-to=origin/dev dev
$ git pull   			//解决冲突后，
$ git commit 
$ git push origin dev
```

#### 查看log（图形化）
`git log --graph --pretty=oneline --abbrev-commit
`

#### 给每个commit创建一个tag
```
$ git tag v1.0					//默认给最新的commit打标签
$ git tag v2.0 <commit-id>		//给特定id的commit打标签
```

#### git rebase use
```
$ git rebase -i  [startpoint]  [endpoint]
```
其中-i的意思是--interactive，即弹出交互式的界面让用户编辑完成合并操作，[startpoint]  [endpoint]则指定了一个编辑区间，如果不指定[endpoint]，则该区间的终点默认是当前分支HEAD所指向的commit(注：该区间指定的是一个前开后闭的区间)。
> git rebase 可以使我们的commit历史变得很清晰。用于合并多个commit。
> 但是你应该只使用 rebase 来清理你的本地工作，千万不要尝试着对那些已经被发布的提交进行这个操作。
> 多人合作时，git rebase指令要慎用，当然如果你自己有一个单独的分支，那么你在那个分支上进行git rebase 是可以的，用于自己将自己的commit整理等。
注意： 当git rebase 出现问题时，应该多到网上查询答案。

```
$ git rebase   [startpoint]   [endpoint]  --onto  [branchName]
```
将某一段commit粘贴到另一个分支上

Note: 在与多人合作时，一定要慎用 --force 参数以及  rebase 指令。