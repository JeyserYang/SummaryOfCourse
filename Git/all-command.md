## 命令总结

#### 创建新目录，进入目录
```
$ mkdir learngit
$ cd learngit
```
#### 生成新仓库

`$ git init`

#### 添加文件到git库中

`$ git add readme.txt`

#### 把文件提交给仓库

`$ git commit -m "wrote a readme file"`

#### 查看工作区状态

`$ git status`

#### 查看文件的修改内容

`$ git diff`

#### git log 查看每次提交(commit)的内容,如果嫌输出信息太多，看得眼花缭乱的，可以试试加上--pretty=oneline参数：

`$ git log --pretty=oneline`

#### HEAD指向的版本就是当前版本，因此，Git允许我们在版本的历史之间穿梭，

`$ git reset --hard commit_id。`

#### 要重返未来，用git reflog查看命令历史，以便确定要回到未来的哪个版本。

`$ git reflog`

#### 概念

工作区：我们当前所在目录，比如learngit文件夹就是一个工作区

版本库(repository)：工作区有一个隐藏目录.git，这个不算工作区，而是版本库。

#### 场景1：当你改乱了工作区某个文件的内容，想直接丢弃工作区的修改时

`$ git checkout -- file。`

#### 场景2：当你不但改乱了工作区某个文件的内容，还添加到了暂存区时，想丢弃修改，分两步，第一步用命令git reset HEAD <file>，就回到了场景1，第二步按场景1操作。
用命令git reset HEAD <file>可以把暂存区的修改撤销掉（unstage），重新放回工作区
git reset命令既可以回退版本，也可以把暂存区的修改回退到工作区。当我们用HEAD时，表示最新的版本

#### 场景3：已经提交了不合适的修改到版本库时，想要撤销本次提交，使用版本回退，不过前提是没有推送到远程库

#### 删除文件
一般情况下，你通常直接在文件管理器中把没用的文件删了，或者用rm命令删了：

`$ rm test.txt`

从版本库中删除该文件，那就用命令git rm删掉，并且git commit：
```
$ git rm test.txt
$ git commit -m "remove test.txt"
```
#### 假如你已经在本地创建了一个git仓库，又想在GitHub上创建一个git仓库，并让这两个仓库同步。首先链接GitHub上的仓库，然后将本地仓库的所有内容推送到远程库中。
```
$ git remote add origin git@github.com:JeyserYang/learngit.git
$ git push -u origin master
```
其中origin代表远程库，master代表分支，只要你在本地作了提交，就可以输入

`$ git push origin master`

#### clone远程库
```
$ git clone git@github.com:JeyserYang/learngit.git
$ cd learngit
$ ls
$ README.md
```
#### 分支管理
```
$ git branch							//查看分支
$ git branch <name>						//创建分支
$ git checkout <name>					//切换分支
$ git checkout -b <name>				//创建+切换分支
$ git merge <name>						//合并分支到当前分支
$ git branch -d <name>					//删除分支
```
#### 当Git无法自动合并分支时，就必须首先解决冲突。解决冲突后，再提交，合并完成。解决冲突就是把Git合并失败的文件手动编辑为我们希望的内容，再提交。用git log --graph命令可以看到分支合并图。

`$ git log --graph --pretty=oneline --abbrev-commit`

#### 合并分支时，加上--no-ff参数就可以用普通模式合并，合并后的历史有分支，能看出来曾经做过合并，而fast forward合并就看不出来曾经做过合并。

`$ git merge --no-ff -m "merge with no-ff" <name>`

其中-m与commit中一样，表示描述信息。

#### 软件开发中，bug就像家常便饭一样。有了bug就需要修复，修复一个代号101的bug的任务时，很自然地，你想创建一个分支issue-101来修复它，修复bug时，我们会通过创建新的bug分支进行修复，然后合并，最后删除。

#### 当手头工作没有完成时，先把工作现场git stash一下，然后去修复bug，修复后，再git stash pop，回到工作现场。
```
$ git stash  							//将工作现场储存起来
$ git stash list						
$ git stash pop 						//恢复现场，同时把stash内容删除
`$ git stash apply						//恢复现场，不把stash内容删除
```
#### 添加一个新功能时，你肯定不希望因为一些实验性质的代码，把主分支搞乱了，所以，每添加一个新功能，最好新建一个feature分支，在上面开发，完成后，合并，最后，删除该feature分支

#### 如果要丢弃一个没有被合并过的分支，可以通过git branch -D <name>强行删除

`$ git branch -D <name>`

#### 现实远程库的详细信息

`$ git remote -v`

#### 抓取分支(pull),当我们从远程库clone时，默认只克隆了master分支，如果想要克隆远程库中的其他分支，比如dev分支，用以下命令

`$ git branch -r`		or 		`$ git branch -a`		//查看远程库分支
`$ git checkout -b <branch_name>`	

#### 当你push的内容和别人push的内容又冲突时，先用git pull把最新的提交从origin/dev抓下来，然后，在本地合并，解决冲突，再推送。在此之前，需要指定本地dev分支与远程origin/dev分支的链接，根据提示，设置dev和origin/dev的链接
```
$ git branch --set-upstream-to=origin/dev dev
$ git pull   			//解决冲突后，
$ git commit 
$ git push origin dev
```
#### rebase操作可以把本地未push的分叉提交历史整理成直线；

`$ git rebase`

#### 给每个commit创建一个tag
```
$ git tag v1.0					//默认给最新的commit打标签
$ git tag v2.0 <commit-id>		//给特定id的commit打标签
```
#### 查看标签信息

`$ git show <tagname>`

#### 创建带有说明的tag

`$ git tag -a v1.0 -m "version 0.1 released" <commit-id>`

#### 删除标签

`$ git tag -d v1.0`

#### 推送本地标签
```
$ git push origin v1.0
$ git push origin --tags			//一次性推送全部标签
```
#### 删除远程标签
```
$ git tag -d v1.0						//首先删除本地标签
$ git push origin :refs/tags/v1.0		//删除远程标签
```
#### 使用.gitignore文件忽略不想push到远程库的内容,例如添加以下这种文件内容。最后将.gitignore文件也push到远程库中去。
```
Windows:
Thumbs.db
ehthumbs.db
Desktop.ini

Python:
*.py[cod]
*.so
*.egg
*.egg-info
dist
build

My configurations:
db.ini
deploy_key_rsa
```
#### 配置别名，例如我想要个status配置别名st
```
$ git config --global alias.st status
$ git config --global alias.co checkout
$ git config --global alias.ci commit
```
甚至还有人丧心病狂地把lg配置成了
```
$ git config --global alias.lg "log --color --graph --pretty=format:'%Cred%h%Creset -%C(yellow)%d%Creset %s %Cgreen(%cr) %C(bold blue)<%an>%Creset' --abbrev-commit"
```

也可以在.git/config目录下找到这些配置，并手动修改。


