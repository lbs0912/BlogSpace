---
title: Git Notes
date: 2017-02-09 14:21:00
tags: [Git,GitHub,Notes]
categories: Git
---

> Git is a free and open source distributed version control system designed to handle everything from small to very large projects with speed and efficiency. --- [Git](https://git-scm.com/)

* 本文主要记录Git学习和使用过程中遇到的问题和知识点。
* [Git Official Website](https://git-scm.com/)
* [Git 教程——廖雪峰](http://www.liaoxuefeng.com/wiki/0013739516305929606dd18361248578c67b8067c8c017b000)

<!-- more -->


## 更新日志
* 2015/08/12: Git学习笔记撰写
* 2016/04/21: Git使用常见问题记录
* 2017/02/09: Git文章整理和重排

## 版本控制系统
* 集中式：`CVS`, `SVN`
* 分布式： `Git`

## Git安装与配置

###  Linux下安装Git
```
sudo apt-get install git
```

### windows下安装Git
下载[msysgit](https://git-for-windows.github.io/)进行安装。`msysgit`是windows版的git，内部集成了`Cygwin`模拟环境和git。

###  配置账户与密码
```
git config --global user.name "Your Name"
git config --global user.email "email@example.com"
```
> `git config`命令的`--global`参数表示对机器上所有的Git仓库都使用该配置。当然也可以对某个仓库指定不同的用户名和Email地址。

## Git基础操作

### 创建仓库并添加文件 
```
git init
git add filename
git commit -m "write a readme file"
git status   //查看当前仓库状态
```

### 查看仓库当前状态，修改内容和历史记录
```
git status   //查看当前仓库状态
git diff     //查看修改内容
git log     //查看仓库历史记录
git log --pretty=oneline      //单行信息输出历史记录   提交历史
git reflog    //记录用户每次输入的命令行   命令历史
```

###  版本回退
```
git reset --hard HEAD^   //退回上一个版本
git reset --hard HEAD^^  //退回上上一个版本
git reset --hard HEAD^^^  //退回上上上一个版本
git reset --hard HEAD~100  //退回上100个版本
```
Git中，`HEAD`表示当前版本，`HEAD^`表示上一个版本，`HEAD^^`表示上上一个版本，`HEAD~100`表示上100个版本版本。

也可以在版本退回中使用`commit id`指定退回的版本。`commit id`为`git log`输出的日志信息中的一串数字。版本退回时，版本号没有必要写全，Git会根据输入的信息自动查找指定的版本。
```
$ git log --pretty=oneline
3628164fb26d48395383f8f31179f24e0882e1e0 append GPL
ea34578d5496d7dd233c827ed32a8cd576c5ee85 add distributed
cb926e7ea50ad11b8f9e909c05226233bf755030 wrote a readme file

$ git reset --hard 3628164   //git reset --hard commit_id
HEAD is now at 3628164 append GPL
```

###  工作区和暂存区
参见[Git 教程——廖雪峰](http://www.liaoxuefeng.com/wiki/0013739516305929606dd18361248578c67b8067c8c017b000)了解更多。
###  撤销修改
```
//场景1： 撤销工作区的修改
git checkout --file

//场景2： 撤销已经添加到暂存区的修改
git reset HEAD file  //退回场景1
git checkout --file

//场景3： 撤销已经commit的修改（前提是未推送到远程仓库）
//参考上一节`版本回退`处理该情况
```
### 删除文件
* 若用`rm filename`命令只能删除本地仓库工作区的文件，此时工作区和版本库的状态就不一致了，用`git status`会显示哪些文件被删除了。

* 确定从版本库中删除文件
```
git rm filename
git commit -m "delete filename"
```
* 误删文件，从版本库中恢复删除文件到工作区
```
git checkout -- test.txt
```
> `git checkout`其实是用版本库里的版本替换工作区的版本，无论工作区是修改还是删除，都可以“一键还原”。

## 远程仓库

### GitHub设置
* 创建SSH Key

查看`userName/.ssh`目录下是否有`id_rsa`和`id_rsa.pub`这两个文件。若已存在，则直接跳到下一步。否则，执行下述命令，创建SSH Key。
```
 ssh-keygen -t rsa -C "youremail@example.com"
```
* `id_rsa`是私钥，不能泄露。`id_rsa.pub`是公钥，可以放心公开。
* 在[GitHub](https://github.com/)界面注册账号，并添加SSH Key。

### 添加远程仓库
* 关联远程仓库
```
git remote add origin git@server-name:path/repo-name.git
// Or
git remote add origin repositories' address
```
* 将本地仓库文件推送到远程仓库
```
git push -u origin master  //第1次推送
git push origin master  //非第1次推送
```
> 第一次推送`master`分支时，加上了`-u`参数，此时Git不但会把本地的master分支内容推送的远程新的master分支，还会把本地的master分支和远程的master分支关联起来，在以后的推送或者拉取时就可以简化命令。后续推送时，不需使用`-u`参数。

### 从远程库克隆至本地
```
git clone git@github.com:michaelliao/gitskills.git  //ssh协议
//Or
git clone https://github.com/michaelliao/gitskills.git //https协议
```
Git支持多种协议，包括`https`和`ssh`协议。但通过`ssh`支持的原生git协议速度最快。默认的`git://`使用`ssh协议`。

## 分支管理

### 创建与合并分支
Git鼓励大量使用分支进行操作。
* 查看分支： `git branch`。当前使用的分支名称前会标有`*`号。
* 创建分支： `git branch <name>`
* 切换分支： `git checkout <name>`
* 创建并切换分支： `git checkout -b <name>` 
* 合并某分支到当前分支： 
```
git merge <name>
git merge -m "add information" <name>
git merge --no-ff -m "merge with no-ff" <name>
```
* 删除分支： `git branch -d <name>`

### 解决冲突
* 当两个不同的分支指向的仓库状态不同时，合并分支时会发生冲突，使用`git status`可以查看冲突的文件。
* 解决冲突后，再提交，合并完成。
* 用`git log --graph`命令可以看到分支合并图

### 分支管理策略
* 应该保持`master`分支的稳定，如仅用来发布新版本。日常开发和测试工作应在`dev`分支上进行。当产品有新功能上线时，再把dev分支合并到master上。
* `Fast Forward`模式合并和普通模式(禁用FF)合并
```
git merge -m "merge with ff" dev
git merge --no-ff -m "merge with no-ff" dev
```
 `Fast Forward`模式合并分支后，会丢失分支信息。`--no-ff`参数表示进行普通模式合并，合并后历史分支信息会保存。

### Bug分支
`git stash`保存当前dev分支内容，从master分支上创建`issue-01`分支，进行bug修复。修复后提交到master分支上，并切换至dev分支，`git stash apply`恢复之前的工作区内容。
```
//保存dev分支工作现场
git stash
git status   // clean
git stash list  //查看stash list

//切换到master分支，并从master分支上创建并切换到issue-01分支
git checkout master
git checkout issue-01

// fix bug is issue-01 branch

// merge issue-01 to master
git checkout master
git merge --no-ff -m "merged bug fix 01" issue-01
git branch -d issue-01

// 切换到dev分支，并恢复之前的工作现场
git checkout dev
git status   //clean
git stash list  //查看stash list

git stash apply  //恢复stash
git stash drop  // 删除stash

//Or
git stash pop // 恢复并删除stash
```
### Feature分支
* 开发一个新的feature，最好新建一个分支
* 新feature开发完毕后，要将其合并到dev分支上。若要丢弃一个没有被合并过的分支，使用`git branch -d <name>`命令时会给出警告
```
$ git branch -d feature-vulcan
error: The branch 'feature-vulcan' is not fully merged.
If you are sure you want to delete it, run 'git branch -D feature-vulcan'.
```
此时，可以通过`git branch -D <name>`强制删除分支。

### 多人协作

#### 查看远程仓库信息
```
git remote
git remote -v  //显示更详细的信息
```
**从远程仓库克隆时，Git自动把本地的master分支和远程的master分支对应起来了，并且远程仓库的默认名称是`origin`。**
#### 推送分支
* 推送时要指定本地分支。这样，Git就会将该分支推送到远程库对应的远程分支上。
```
//推送本地master
git push origin master  
//推送本地dev分支
git push origin dev

git push origin branch-name
```
* 若推送失败，则先用`git pull`抓取远程的新提交。
* 在本地创建和远程分支对应的分支，本地和远程分支的名称最好一致
```
git checkout -b branch-name origin/branch-name
```
* 建立本地分支和远程分支的关联
```
git branch --set-upstream branch-name origin/branch-name
```

#### 分支推送准则
* 并不是一定要把本地分支推送至远程
* master分支是主分支，因此需要时刻与远程同步
* dev分支是开发分支，团队所有成员都需在上面工作，所以也需要与远程同步
* bug分支只用于本地修复bug，就没必要推到远程了，除非老板要看看你每周到底修复了几个bug
* feature分支是否推到远程，取决于你是否和你的小伙伴合作在上面开发

#### 抓取分支
* 参见[Git 教程——廖雪峰](http://www.liaoxuefeng.com/wiki/0013739516305929606dd18361248578c67b8067c8c017b000/0013760174128707b935b0be6fc4fc6ace66c4f15618f8d000)了解更多
* 建立本地分支和远程分支的关联
```
git branch --set-upstream branch-name origin/branch-name
```
* 从远程抓取分支，使用`git pull`，如果有冲突，要先处理冲突。

####  多人协作工作模式
* 首先，可以试图用`git push origin branch-name`推送自己的修改；
* 如果推送失败，则因为远程分支比你的本地更新，需要先用`git pull`试图合并；
* 如果合并有冲突，则解决冲突，并在本地提交；
* 没有冲突或者解决掉冲突后，再用`git push origin branch-name`推送就能成功;
* 如果`git pull`提示`“no tracking information”`，则说明本地分支和远程分支的链接关系没有创建，用命令`git branch --set-upstream branch-name origin/branch-name`创建二者联系。

## 标签管理
* Git的标签是版本库的一个快照，其本质是指向某个commit的指针。（标签和分支很类似，但是分支可以移动，标签不能移动）

### 创建标签
```
git tag <name>  //Eg. v1.0

//创建带有说明的标签，用-a指定标签名，-m指定说明文字
git tag -a v0.1 -m "version 0.1 released" 3628164

//通过-s用私钥签名一个标签(必须首先安装gpg-GnuPG)
git tag -s v0.2 -m "signed version 0.2 released" fec145a
```
默认的标签是打在最新提交的commit上的。若要对之前的commit打标签，需要先使用`git log`查找到对应的`commit_id`。
```
git log --pretty=oneline --abbrev-commit
git tag <name> commit_id
```
### 查看标签和标签信息
``` 
git tag  //查看标签列表
git show <tagname>  //查看标签信息 
```
### 推送标签到远程
```
git push origin <tagname>  //推送单一tag
git push origin --tags   //推送所有tags
```
### 删除标签
* 标签未被推送至远程，标签在本地
```
git tag -d <name>
```
* 标签已被推送至远程
```
git tag -d v1.0   // local delete
$ git push origin :refs/tags/v1.0  //remote delete
```

## 使用GitHub
* 在GitHub上，可以任意`Fork`开源仓库
* 自己拥有Fork后的仓库的读写权限
* 可以推送`pull request`给官方仓库来贡献代码

## 自定义Git
```
git config --global user.name "Your Name"
git config --global user.email "email@example.com"
git config --global color.ui true  //开启多种颜色支持
```
> `git config`命令的`--global`参数表示对机器上所有的Git仓库都使用该配置。当然也可以对某个仓库指定不同的用户名和Email地址。

### 忽略特殊文件
* 忽略某些文件时，需要编写`.gitignore`
* `.gitignore`文件本身要放到版本库里，并且可以对`.gitignore`进行版本管理
* **忽略文件的原则**
  - 忽略操作系统自动生成的文件，比如缩略图等
  - 忽略编译生成的中间文件、可执行文件等，如Java编译产生的`.class`文件
  - 忽略你自己的带有敏感信息的配置文件，比如存放口令的配置文件
* GitHub已经为我们准备了各种配置文件，不需从头写`.gitignore`文件，只需要组合一下就可以使用了。所有配置文件可以在 [gitignore- GitHub](https://github.com/github/gitignore) 获取
* 例如，windows系统下编译python程序时，首先忽略windows自动生成的垃圾文件，然后忽略python编译产生的中间文件，最后忽略自定义的文件。

```
# Windows:
Thumbs.db
ehthumbs.db
Desktop.ini

# Python:
*.py[cod]
*.so
*.egg
*.egg-info
dist
build

# My configurations:
db.ini
deploy_key_rsa
```

* 若一个文件在`.gitignore`中被忽略，则在`git add <name>`时候会报错。
 - `-f`强制提交该文件： `git add -f <name>`
 - 查看提交时涉及到的忽略准则：`git check-ignore`。该指令可以检查哪条准则忽略了该文件。去除该准则后，可以正常提交文件。

### 配置别名
```
git config --global alias.st status
```
将`st`设置为`status`的别名。之后使用`git st`和`git status`效果相同。

### 搭建Git服务器
搭建Git服务器需要一台运行Linux的机器。参见[Git 教程——廖雪峰](http://www.liaoxuefeng.com/wiki/0013739516305929606dd18361248578c67b8067c8c017b000/00137583770360579bc4b458f044ce7afed3df579123eca000)了解更多。

## Git Cheat Sheet
参考[Git-cheatSheet](http://ol3kbaay9.bkt.clouddn.com/git-cheatsheet.pdf)查阅Git相关操作。

## FAQ

## 参考资料
[1] [Git Official Website](https://git-scm.com/)
[2] [Git 教程——廖雪峰](http://www.liaoxuefeng.com/wiki/0013739516305929606dd18361248578c67b8067c8c017b000)

## 反馈与建议
- 邮箱：<lbs1203940926@163.com>
- 微信：[@脱缰的哈士奇(ab1203940926)](http://ojx8u3g1z.bkt.clouddn.com/wechat-id.jpg)
- 微博：[@脱缰的哈士](http://weibo.com/2329754491/profile) 