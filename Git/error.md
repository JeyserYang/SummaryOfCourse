# 错误总结

#### fatal : refusing to merge unrelated histories

solution：在操作后面添加 `--allow-unrelated-histories`

比如：	`git pull origin master --allow-unrelated-histories`

#### build a yourself blog - github pages & jekyll
solution:
- 首先在自己的电脑上安装jekyll，具体可参见jekyll官网
- 其次在自己的电脑上新建一个本地jekyll site `jekyll new username.github.io`
- 然后启动Jekyll serve  `jekyll serve`
- 此时你登陆`localhost:4000`就可以在本地看到网站内容。
- 在此目录下，创建git 仓库   `git init`
- 然后提交并并和远程你的username.github.io链接
```
git add .
git commit -m "updated site
git remote add origin git@github.com:username/username.github.io
git push -u origin master
```
- 然后你就可以修改里面的配置，以及上传博客，具体里面文件夹的意义可参考jekyll官网

#### git rebase中出现问题
```
Cannot pull with rebase: You have unstaged changes.
Please commit or stash them.
```
> Do git status, this will show you what files have changed. Since you stated that you don't want to keep the changes you can do git checkout -- <file name> or git reset --hard to get rid of the changes.

For the most part, git will tell you what to do about changes. For example, your error message said to  git stash your changes. This would be if you wanted to keep them. After pulling, you would then do  git stash pop and your changes would be reapplied.

git status also has how to get rid of changes depending on if the file is staged for commit or not.

从别人的经验可以看出，当出现问题时，首先**`git status`**，git 自身会给你许多提示，大多数情况下按照提示就可以解决你的问题。