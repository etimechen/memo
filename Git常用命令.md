#Git常用命令

* 创建

    克隆一个现有的版本库 `$ git clone ssh://user@domain.com/repo.git` 或者 `$ git clone git@domain.com/repo.git`

    创建一个新的本地版本库 `$ git init`

* 本地修改

    显示工作区已修改过的文件 `$ git status`

    显示修改内容 `$ git diff`

    添加所有当前修改到暂存区 `$ git add .`

    添加指定文件到暂存区 `$ git add -p <file>`

    提交所有 `$ git commit -a`

    快速提交 `$ git commit`

    修改上一次的提交 `$ git commit --amend` _不要用amend已发布的提交!_

* 提交历史

    显示所有的提交,从最新的开始 `$ git log`

    显示历史命令 `$ git reflog`

    显示指定文件每次提交修改过的内容 `$ git log -p <file>`

    谁在什么时候修改了指定文件的什么内容 `$ git blame <file>`

* 分支和标签

    列出所有当前分支 `$ git branch -av`

    切换HEAD分支 `$ git checkout <branch>`

    基于当前HEAD创建一个新的分支 `$ git branch <new-branch>`

    基于一个远程分支创建一个本地新的跟踪分支 `$ git checkout --track <remote/branch>`

    删除一个本地的分支 `$ git branch -d <branch>`

    给当前提交打上标签 `$ git tag <tag-name>`

* 更新和发布

    列出所有当前远程的配置 `$ git remote -v`

    显示一个远程的信息 `$ git remote show <remote>`

    添加一个新的远程库 `$ git remote add <shortname> <url>`

    从远程库下载所有的修改,但不切换到HEAD指向的分支 `$ git fetch <remote>`

    从远程库下载所有修改,合并/切换到HEAD指向的分支 `$ git pull <remote> <branch>`

    发布本地修改到远程库 `$ git push <remote> <branch>`

    删除远程库上得一个分支 `$ git branch -dr <remote/branch>`

    发布你的标签 `$ git push --tags`

* 合并和rebase

    在你当前HEAD指向分支里合并分支 `$ git merge <branch>`

    在分支上rebase你的当前HEAD指向分支 `$ git rebase <branch>` _不要rebase已发布的提交!_

    取消一个rebase `$ git rebase --abort`

    解决冲突后继续一个rebase `$ git rebase --continue`

    用你配置的合并工具解决冲突 `$ git mergetool`

    使用你的编辑器手工解决冲突,解决后保存为一份已解决冲突的文件 `$ git add <resolved-file>` `$ git rm <resolved-file>`

* 撤销
    
    丢弃工作区所有的本地修改 `$ git reset --hard HEAD`

    回退到上一版本 `$ git reset --hard HEAD^`

    回退到上上一版本 `$ git reset --hard HEAD^^`

    回退到上N个版本 `$ git reset --hard HEAD~N`

    回退到指定提交id版本 `$ git reset --hard <commit>`

    丢弃指定文件的本地修改 `$ git checkout HEAD <file>`

    还原一个提交 `$ git revert <commit>`

    保存所有未标识的改变 `$ git reset <commit>`

    保存所有本地未提交的改变 `$ git reset --keep <commit>`

* 其它

    生成ssh key `$ ssh-keygen -t rsa -C "youremail@example.com"`

    搭建git远程库 `$ git init --bare sample.git` _最好给sample.git加入单独的用户和用户组权限防止其他用户修改工作区_ 

    本地仓库与远程仓库关联 `$ git remote add origin git@domain.com/sample.git`

    






