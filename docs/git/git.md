# Git 命令

## git log 
> 查看全部提交历史的详细信息


## git log --oneline
> 查看全部提交历史的简要信息

## git log --oneline --graph
> 查看版本路线

## git log --author=<用户名>
> 查看某个用户的提交历史

## git reflog
> 查看本地所有历史记录

## git status
> 查看文件、文件夹在工作区、暂存区的状态

## git config --global user.name <用户名>
> 配置git的用户名

## git config --global user.email <邮箱>
> 配置git的邮箱

## git config --global --list
> 查看git账户信息

## git rm <文件路径>
> 删除文件

## git mv <改动前文件名> <改动后文件名>
> 文件重命名

## git mv <文件名> <文件夹>
> 移动文件到指定文件夹下

## git mv <文件名> <文件夹/文件名>
> 移动文件到指定文件夹下并重命名

## git log -p <文件名>
> 查看文件前后变化

## git checkout -- <文件名>
> 文件还原上一次提交的状态（文件不能在暂存区，否则无法还原）

## git add ./<文件名>
> 将文件加入到暂存区中，实现追踪

## git reset HEAD <文件名>
> 将文件从暂存区中撤销出来

## git reset --hard HEAD^
> 项目回到上一个版本

## git reset --hard <提交ID>
> 项目回到指定的版本

## git checkout <提交ID> -- <文件名>
> 指定文件回到指定的版本

## git tag <标签名>
> 给最新的一次提交创建标签

## git tag
> 查看所有标签

## git tag <标签名> <提交版本ID>
> 给指定id的提交创建标签

## git tag -d <标签名>
> 删除指定的标签

## git push origin <标签名>
> 将指定的标签推送到远程

## git branch <分支名>
> 创建分支

## git branch
> 查看本地所有分支

## git branch -av
> 查看本地分支与远程分支的关系

## git checkout <分支名>
> 切换到指定的分支

## git branch -d <分支名>
> 删除指定的分支

## git checkout -b <分支名>
> 创建并切换到指定的分支

## git checkout -b <本地分支> <远程分支>
> 创建切换到本地分支，并与远程分支做关联

## git merge <分支名>
> 将此分支合并到当前的分支

## git merge --abort
> 合并冲突时，忽略合并过来的分支代码，保留原分支代码

## git push origin --delete <分支名>
> 删除远程分支

## git fetch
> 从远程获取最新版本到本地，但不会自动合并（取回的代码对你本地的开发代码没有影响）

## git pull
> 从远程拉取最新版本到本地并自动merge本地代码

## git remote -v
> 查看关联的所有远程仓库名称及地址

## git remote add origin <远程仓库地址>
> 与远程仓库做关联

## git push -u origin master
> 提交本地仓库到远程（-u 是设置默认的推送地址，下次可以直接使用git push命令）

## git stash
> 将修改的内容暂存，并让本地代码回退到修改前的状态

## git stash apply
> 将暂存的内容恢复

## git diff
> 查看当前文件哪些做了修改
