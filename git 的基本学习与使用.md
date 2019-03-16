# git 的学习与使用

##### git 常用命令:

* http://www.ruanyifeng.com/blog/2015/12/git-cheat-sheet.html



##### 创建本地仓库

* git init   -- 初始化本地仓库
* git add .   -- 添加所有文件到本地仓库
* git add 文件名   -- 添加单个文件到本地仓库
* git commit -m '提交日志'   -- 把代码提交到本地仓库
* git status   -- 查看添加状态
* git log   -- 查看提交详情日志
* git log --oneline   -- 查看提交日志
* git rm 文件名   -- 删除单个文件

###### 创建分支

* git branch 分支名   -- 创建分支
* git branch   -- 查看分支
* git checkout 分支名   -- 切换分支
* git checkout -   -- 切换到上一个分支
* git branch -d 分支名   -- 删除本地分支
* git merge 分支名   -- 合并分支

###### 关联远程仓库

* git remote add origin 远程仓库的地址   -- 指定远程仓库
* git remote rm origin 远程仓库的地址   -- 删除远程仓库

###### 远程仓库

* git push -u origin master   -- 把本地代码提交到远程仓库主分支
* git push -u origin 分支名   -- 把本地代码提交到指定分支
* git reset --hard HEAD^   -- 回退到上一个版本
* git reset --hard 版本号   -- 回退到指定版本
* git reflog   -- 查看所有版本



#####  拉取和更新

* git clone 仓库地址   -- 克隆远程仓库到本地
* git pull   -- 把远程代码更新 (把本地代码上传到GitHub)
* git push   -- 把本地代码更新 (把GitHub的代码更新到本地) 

