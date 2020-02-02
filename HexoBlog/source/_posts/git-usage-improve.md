---
title: Git 进阶使用
date: 2020-02-01 10:23:34
categories: Develop Tools
tags: [Develop Tools,Git]
keywords: Develop Tools,Git
toc: true
password:
comments: true
top:
---


* 总结日常开发中的 Git 进阶使用
* 记录多 `SSH` 配置，`git reflog` 解决 `detached-head` 代码丢失问题
* 总结团队协作下，如何保持 git 提交信息简洁

<!--more-->

## Changelog
* 2018/07/09，撰写
* 2019/03/10，添加多SSH配置
* 2019/04/26，添加 `git cherry-pick`
* 2019/10/07，添加如何删除git所有历史提交信息
* 2019/10/14，添加 `git merge --no-ff`
* 2019/12/14，添加 `git reflog` 使用
* 2020/02/01，添加 *删除所有历史提交记录*
* 2020/02/01，添加 *团队协作下，如何保持 git 提交信息简洁*


## Ref
* [GitUp](http://gitup.co/)
* [如何高效使用github](https://www.zhihu.com/question/20070065/answer/480261314)




## git reflog 解决提交代码丢失 `detached-head`

`reflog` 是 Git 操作的一道安全保障，它能够记录几乎所有本地仓库的改变。包括所有分支 commit 提交，已经删除（其实并未被实际删除）commit 都会被记录。总结而言，只要 HEAD 发生变化，就可以通过 `reflog` 查看到。

### `detached-head` 代码丢失找回
* [git提交到HEAD detached导致代码丢失](https://www.jianshu.com/p/f247a27851fb)
* [StackOverflow - gitx How do I get my 'Detached HEAD' commits back into master](https://stackoverflow.com/questions/4845505/gitx-how-do-i-get-my-detached-head-commits-back-into-master)

#### 背景
日常开发中，切换分支误操作，造成本地代码修改丢失。

此时，可以借助 `git reflog` 找回丢失的代码修改。


#### 丢失产生原因和步骤

首先在 `master` 分支上开发，此时线上出现 bug 且回到旧版本的 tag。这时 `master` 分支上有一部分代码修改但未提交。

在 `master` 分支上执行 `git status`，有未提交的代码，如下图所示

![](https://image-bed-20181207-1257458714.cos.ap-shanghai.myqcloud.com/programming-2019/git-reflog-1.png)


在 `master` 分支上执行 `git tag`查看标签信息，如下图所示

![](https://image-bed-20181207-1257458714.cos.ap-shanghai.myqcloud.com/programming-2019/git-reflog-2.png)

此时有未提交的代码，然后执行 `git checkout v1.0`

![](https://image-bed-20181207-1257458714.cos.ap-shanghai.myqcloud.com/programming-2019/git-reflog-3.png)

这个时候，**提示当前分支为 `detached HEAD`**

然后再执行 `git add ./git commit` 和 `git checkout master`，切换回 `master` 分支。**这个时候发现 `detached HEAD` 分支不见了，`master` 分支上未提交的代码也不见了。**



#### 代码找回

执行 `git reflog` 查看提交记录

![](https://image-bed-20181207-1257458714.cos.ap-shanghai.myqcloud.com/programming-2019/git-reflog-4.png)

查找对应提交的 `commitId` 为 `247e11b`，然后执行下述命令行，找回丢失的代码

```
git checkout 247e11b    //检出对应的提交
git checkout -b diff    //新建一个新的diff分支
git checkout master     //切换到master分支
git merge diff          //将新建的diff分支合并到master分支
```


## 删除所有历史提交记录
* [How to delete all commit history in github | Stackoverflow](https://stackoverflow.com/questions/13716658/how-to-delete-all-commit-history-in-github)

此处介绍如何删除所有历史提交记录，形成一个全新的仓库。

* 1 - Checkout

```
git checkout --orphan new_branch
```

* 2 - Add all the files

```
git add -A

//等效于 git add --all 或 git add .
```

>  `git add` 中使用参数 `-A` 或 `--all` 表示追踪所有操作，包含新增、修改和删除
>
>  Git 2.0版开始，`-A` 参数为默认参数，即 `git add .` 等效于 `git add -A` 或 `git add --all`

* 3 - Commit the changes

```
git commit -am "commit message"
```


* 4 - Delete the branch

```
git branch -D master   //同时删除本地和远程分支
```

* 5 - Rename the current branch to master

```
git branch -m master
```

* 6 - force update your repository

```
git push -f origin master
```


下面对上述步骤进行说明

### git checkout --orphan

如果你的某个分支上积累了无数次的无意义的提交，`git log` 信息满天飞，那么可以使用 `git checkout --orphan <new_branch_name>` 

* 基于当前分支创建一个新的“孤儿(`orphan`)”的分支，没有任何提交历史，但包含当前分支所有内容
* 执行上述命令后，工作区（`Workspace`）中所有文件均被认为在该操作中新增(`git statue` 查看状态，所有文件状态均为 `new file`，如下图所示)，此时执行 `git add .` 会把所有文件添加到缓存区（`Index`）

![](https://image-bed-20181207-1257458714.cos.ap-shanghai.myqcloud.com/programming-2019/git-checkout-orphan-1.png)

* 严格意义上说，执行 `git checkout --orphan <new_branch_name>` 后，创建的并不是一个分支，因为此时 `HEAD` 指向的引用中没有 `commit` 值。只有在进行一次提交后，它才算得上真正的分支。

> `orphan` 译为“孤儿”，该参数表示创建一个孤立的分支，没有任何提交历史，且与当前分支不存在任何关系（查看提交信息，可发现其为一个孤立的点，如下图所示）
>
> 孤儿（`orphan`）无父辈信息，同理，创建的分支也不包含任何历史提交信息



![](https://image-bed-20181207-1257458714.cos.ap-shanghai.myqcloud.com/programming-2019/git-delete-all-info-1.png)


### git commit -am

* [git commit -m 与 git commit -am 的区别](https://segmentfault.com/q/1010000005900988)


### git branch -m
重命名


### git push -f origin master

## git项目协作——保证git信息简洁
### 同一分支 git pull 使用 rebase
默认情况下，`git pull` 使用的是 `merge` 行为。多人协作开发时，会产生不必要的 `merge` 提交记录，造成提交链混乱不堪。

推荐在同一个分支更新代码时，使用 `git pull --rebase`。

```
# 为某个分支单独设置，这里是设置 dev 分支
git config branch.dev.rebase true
# 全局设置，所有的分支 git pull 均使用 --rebase
git config --global pull.rebase true
git config --global branch.autoSetupRebase always
```
### 分支合并使用 merge --no-ff

#### Usage
* `Fast-Forward`：当前分支合并到另一分支时，如果没有冲突要解决，就会直接移动文件指针，并且不会产生合并提交记录。该过程中，存在`git` 文件指针快速移动， 因此该过程称为 `Fast-Forward`。
* `--no-ff`(`no fast foward`)：每一次的合并，都会创建一个新的 `commit` 记录。使用 `--no-ff`，可以保持原有分支提交链的完整性，并且当该分支被删除时，提交信息依旧存在。

![git-merge--no-ff-1](https://image-bed-20181207-1257458714.cos.ap-shanghai.myqcloud.com/programming-2019/git-merge--no--ff-1.png)

结合上图分析，在 `dev`（绿色） 分支上检出 `feature-1` 分支（蓝色），且 `dev` 分支不进行任何提交
* 直接 `merge`，默认采用 `Fast-Forward`，两个分支的提交链会合并为一条直线，不利于后期代码审查和维护
* 使用 `git merge --no-ff feature-1` 合并代码，会产生一个新的提交，且两个分支的提交链不会重叠，利于后期代码审查和维护

#### merge 默认设置

`git merge` 默认使用 `fast-forward`，可以通过如下方式，修改为默认使用 `--no-ff`。

```
git config --global merge.commit no
git config --global merge.ff no
```

此外，SourceTree 在设置中也可以设置 `--no-ff`。

![git-merge--no-ff-2](https://image-bed-20181207-1257458714.cos.ap-shanghai.myqcloud.com/programming-2019/git-merge--no-ff-2.png)







## IDE中使用Git

### VSCode中使用Git

* [VSCode 中使用Git实践](https://juejin.im/post/5b00474951882542ba08087a)
* 推荐安装 `Git Lens` 和 `Git History` 插件
* 克隆代码
    + `Ctrl + Shift + P` 打开命令面板，输入 `Git`，选择 `Git Clone` 进行克隆代码
* 查看修改（VSCode会使用不同颜色进行标识）
    + 红色箭头 - 标识删除行
    + 蓝色竖线 - 该处有修改
    + 绿色箭头 - 该处为新增

![](https://image-bed-20181207-1257458714.cos.ap-shanghai.myqcloud.com/programming-2019/git-vscode-usage-1.png?q-sign-algorithm=sha1&q-ak=AKIDnYCjAUImfNuHSSn8nHaihXEkvukPOAGM&q-sign-time=1570451528;1570453328&q-key-time=1570451528;1570453328&q-header-list=&q-url-param-list=&q-signature=b0ec61fae96fb0c37059f2afedbac8e2ad1b2753)

* 提交代码
    + `Ctrl + Shift + G` 打开代码管理器进行操作

![](https://image-bed-20181207-1257458714.cos.ap-shanghai.myqcloud.com/programming-2019/git-vscode-usage-2.png?q-sign-algorithm=sha1&q-ak=AKIDnYCjAUImfNuHSSn8nHaihXEkvukPOAGM&q-sign-time=1570453094;1570454894&q-key-time=1570453094;1570454894&q-header-list=&q-url-param-list=&q-signature=63ea253d235cba5fc2d3d5c525e5242d11bba407)

## git clone 设置缓存区

当工程较大时，使用 `git clone` 拉取代码，可能会出现 `early EOF` 的报错或者拉取代码失败。

这是因为 `git clone` 本质上是建立一个 HTTP 连接，工程较大时会超过默认设置的缓存大小。


使用 `git config --list` 查看 `http.postbuffer` 的大小，确认是否小于下载的工程大小。

使用 `git config --global http.postbuffer 524288000 //500x1024x1024 设置为500M` 可以对缓存区大小进行设置。 



## git clean
* `git clean -n` -  查看哪些文件将被 `git clean` 清除，只是查看，并不会真正执行清除操作
* `git clean -f`  - 删除未跟踪的文件 `untracked files` 
* `git clean -fd` - 连同未跟踪的目录也一起删除  
* `git clean -fdx` - 删除未跟踪的文件和文件目录，并移除被忽略的文件。其中，`-x` 表示移除被忽略的文件并且 `.gitignore` 文件中指定的文件和文件夹也会清除或者清除更改

## git tag

* `git tag`： 显示所有标签
* `git tag -l 'v1.0.*'`： 用通配符查看符合筛选条件的标签
* `git show xxx`： 查看标签信息（提交者，邮箱等）
* `git tag xxx`： 创建轻量标签
* `git tag -a xxx`： 创建含有附注的标签
* `git tag -a xxx -m 'xxxx'`： 创建含有附注的标签，并附加提交信息（默认标签打到当前Head提交状态）
* `git tag -a xxx -m 'xxxx' \<commitID>`： 创建补丁标签，即对之前的提交添加标签
* `git tag -d xxx`： 删除本地标签
* `git push origin --delete tag <tagname>`： 删除远程标签


需要注意的是，
Git 使用的标签有 2 种类型：轻量级的（`lightweight`）和含附注的（`annotated`）。 —— [git tag | Doc](https://git-scm.com/book/zh/v1/Git-%E5%9F%BA%E7%A1%80-%E6%89%93%E6%A0%87%E7%AD%BE)

* 轻量级标签就像是个不会变化的分支，实际上它就是个指向特定提交对象的引用。
* 含附注标签，实际上是存储在仓库中的一个独立对象，它有自身的校验和信息，包含着标签的名字，电子邮件地址和日期，以及标签说明，标签本身也允许使用 `GNU Privacy Guard` (`GPG`) 来签署或验证。

**一般我们都建议使用含附注型的标签，以便保留相关信息；当然，如果只是临时性加注标签，或者不需要旁注额外信息，用轻量级标签也没问题。**



## cherry-pick
* [Cherry-Pick | 掘金](https://juejin.im/post/5925a2d9a22b9d0058b0fd9b)

### 使用场景
`cherry` 译为樱桃，`pick` 译挑选。`git cherry-pick` 即选择某一个分支中的一个或几个提交，合并到其他分支中（选择的提交即所需的樱桃），主要使用场景为
* 情况1： 把弄错分支的提交移动到正确的分支上
* 情况2： 将其他分支的提交添加到当前分支

### Demo

假设工程有个稳定版分支 `v2.0`，还有个开发版分支 `v3.0`。开发分支还未彻底完成，不能直接把两个分支合并，这样会导致稳定版本混乱，但是又想增加一个 `v3.0` 中的功能到 `v2.0` 中，这里就可以使用 `cherry-pick` 了。


```
// 先在v3.0中查看要合并的commit的commit id
git log
// 假设是 commit f79b0b1ffe445cab6e531260743fa4e08fb4048b

// 切到v2.0中
git checkout v2.0

// 合并commit
git cherry-pick f79b0b1ffe445cab6e531260743fa4e08fb4048b     
git cherry-pick -x f79b0b1ffe445cab6e531260743fa4e08fb4048b   //表示保留原提交的作者信息进行提交
```

### 语法
* `git cherry_pick commitID`：将其他分支的 `commitID` 提交合并到当前分支
* `git cherry_pick commitID`：将其他分支的 `commitID` 提交合并到当前分支，`-x` 表示保留原提交的作者信息进行提交
* `git cherry_pick <start-commit-id>…<end-commit-id>`: 该功能在Git 1.7.2 版本后才支持，将一个连续区间范围的提交，合并到到当前分支。提交范围区间左开右闭，即`(start, end]`
* `git cherry_pick <start-commit-id>^ … <end-commit-id>`: 同上，使用 `^` 表示包含 `start-commit-id`，即`[start, end]`


> JetBranins 系列IDE，内置了git cherry-pick 快捷键（樱桃图标）





## git命令行代理设置
### 设置代理

```
git config --global http.proxy 'socks5://127.0.0.1:1086'
git config --global https.proxy 'socks5://127.0.0.1:1086'
```

### 取消代理

```
git config --global --unset http.proxy
git config --global --unset https.proxy
```


## GitUp

> Work quickly, safely, and without headaches.
The Git interface you've been missing
all your life has finally arrived.

* [GitUp](http://gitup.co/)
* [Tutorial](http://qinghua.github.io/gitup/)


## GitG

* `gitg`是一个git图形化界面。
* 安装

```
brew install gitg   //安装
```

* 使用


```
gitg  // 在目录终端下输入gitg即可
```

## Git 代码回滚

* [谈谈 Git 代码回滚](https://sunmengyuan.github.io/garden/2017/06/15/git-revert.html?page=2)


### 场景描述
某项目，并行开发着n个需求。提测时，各需求的代码被合并到测试分支。不久之后，要求把部分需求代码从测试分支抽离出去。使用下图场景进行描述。并行开发3个需求，分别是`feature1`，`feature2`，`feature3`。测试分支为`master`。


![git-rollback](https://image-bed-20181207-1257458714.cos.ap-shanghai.myqcloud.com/programming-2019/git-rollback-1.jpg)


`feature2`与`feature3`对同一文件进行修改，故意制造一个冲突。 

![git-rollback](https://image-bed-20181207-1257458714.cos.ap-shanghai.myqcloud.com/programming-2019/git-rollback-2.jpg)



提测时，各分支代码被合并到测试分支（`master`）。首先，`featuer1`分支被合并到测试分支。

![git-rollback](https://image-bed-20181207-1257458714.cos.ap-shanghai.myqcloud.com/programming-2019/git-rollback-3.jpg)


之后，`featuer2`分支也被合并到测试分支。

![git-rollback](https://image-bed-20181207-1257458714.cos.ap-shanghai.myqcloud.com/programming-2019/git-rollback-4.jpg)


最后，合并`feature3`至测试分支。合并时，产生了与`feature2`代码的冲突。

![git-rollback](https://image-bed-20181207-1257458714.cos.ap-shanghai.myqcloud.com/programming-2019/git-rollback-5.jpg)


解决冲突之后，继续将`feature3`合并至测试分支。

![git-rollback](https://image-bed-20181207-1257458714.cos.ap-shanghai.myqcloud.com/programming-2019/git-rollback-6.jpg)



在`feature3`提测后，在测试分支上继续修复几个bug。

![git-rollback](https://image-bed-20181207-1257458714.cos.ap-shanghai.myqcloud.com/programming-2019/git-rollback-7.jpg)

注意，此时`feature2`虽已提测但并未进入测试，此时的bug修复均是针对 `feature1`与`feature3`。

几天之后，收到通知，`feature2`的测试无法正常进行，需将代码从测试分支上抽出。


### 代码回滚操作

#### Step 1 切换分支

首先，切换到`featuer2`分支。以防万一，创建`feature2-copy`分支，对该分支进行备份。

```
git checkout feature2   //切换到feature2分支

git checkout -b feature2-copy   //创建并切换到feature2-copy

//第2行代码等同于
git branch feature2-copy  //创建feature2-copy分支
git checkout feature2-copy //切换到feature2-copy分支
```

![git-rollback](https://image-bed-20181207-1257458714.cos.ap-shanghai.myqcloud.com/programming-2019/git-rollback-8.jpg)


#### Step 2 确定要回滚的提交记录

使用`git log`查看`feature2-copy`分支的提交记录（输入`q`退出`git log`环境）。

```
git log
```
![git-rollback](https://image-bed-20181207-1257458714.cos.ap-shanghai.myqcloud.com/programming-2019/git-rollback-9.jpg)


如图所示，需要回滚最新的3个提交。实际情况中，针对某需求的提交绝不止3个。若是将提交逐一`revert`，工作量是非常大的。需要考虑将`n`个`commit`合并为一个`commit`，最后一同`revert`。



#### Step 3 git rebase 合并提交
使用`git rebase -i`来合并`commit`，传入需要拼接回滚至的提交的`hashcode`。（此处，将所有回滚的提交合并到需要回滚的`commit`集合中第一个提交）

```
//hashcode为需要回滚的commit集合中第一个提交
git rebase -i e08ddaf558b9ad84422db5e4b620dcab97623fde
```

![git-rollback](https://image-bed-20181207-1257458714.cos.ap-shanghai.myqcloud.com/programming-2019/git-rollback-10.jpg)

将最近2次提交的`command`从`pick`改为`s`。

> 在Vim中，
>   1. 输入`i`，进入`INSERT`模式。
>   2. 输入`ESC`，进入命令行模式。
>   3. 输入`:wq`，保存并退出VIM编辑器。


![git-rollback](https://image-bed-20181207-1257458714.cos.ap-shanghai.myqcloud.com/programming-2019/git-rollback-11.jpg)


修改后，保存并退出，进入如下对话框。

![git-rollback](https://image-bed-20181207-1257458714.cos.ap-shanghai.myqcloud.com/programming-2019/git-rollback-12.jpg)

此时，对最初一次的提交的`commit message`进行修改。


![git-rollback](https://image-bed-20181207-1257458714.cos.ap-shanghai.myqcloud.com/programming-2019/git-rollback-13.jpg)

修改后保存并退出，使用`git log` 再次查看 `feature2-copy` 分支的信息。



![git-rollback](https://image-bed-20181207-1257458714.cos.ap-shanghai.myqcloud.com/programming-2019/git-rollback-14.jpg)


如上图所示，3次提交被成功合并。


#### Step 4 git revert 撤销提交

```
git revert e544464c3de69adef5ca7556001abebaf40b218b
```

![git-rollback](https://image-bed-20181207-1257458714.cos.ap-shanghai.myqcloud.com/programming-2019/git-rollback-15.jpg)

保存并退出，再次查看`feature2-copy`分支的提交记录。

![git-rollback](https://image-bed-20181207-1257458714.cos.ap-shanghai.myqcloud.com/programming-2019/git-rollback-16.jpg)


#### Step 5 git cherry-pick

```
git checkout master   //切换到测试分支
git cherry-pick b309f7944d2422d8fe647dca61bda518b192628f
```

切换到测试分支，并执行 `git cherry-pick` 命令。至此，成功的将`feature2`分支从测试分支上抽离。


![git-rollback](https://image-bed-20181207-1257458714.cos.ap-shanghai.myqcloud.com/programming-2019/git-rollback-17.jpg)


## Git代码管理与团队协作

* [视频教程 | Segmentfault](https://segmentfault.com/l/1500000015442316/play)

![git-team-usage](https://image-bed-20181207-1257458714.cos.ap-shanghai.myqcloud.com/programming-2019/git-team-usage-3.png)

代码最终提交效果如上图所示。

* 主分支为`master`，创建一个`develop`分支用于开发。
* 开发者`leo`和`jack`创建自己的分支`leo`和`jack`进行开发。开发完成后，将其合并到`develop`分支上。
* 项目进展到需要发布时，从`develop`分支创建`release`分支，用于测试。测试通过后，将`release`分支合并到`develop`分支和`master`分支上。


### Demo目录和初始化

```
//目录结构
-- c4
---|--- origin   //模拟远程仓库  git init --bare 创建
---|--- leo    //开发者1  设置user.name = 'leo'
---|--- jack    //开发者2  设置user.name = 'jack'
```

> `git init --bare` 用于创建一个“裸仓库”——只含有 `.git` 目录，不含源文件。详情参考 [Ref](https://segmentfault.com/q/1010000004683286)。

```
mkdir c4
cd c4
mkdir origin 
cd origin 
git init --bare   //创建一个裸仓库

cd ..

//克隆远程仓库至两个开发者目录——leo, jack
git clone origin leo
git clone origin jack
```

为了模拟团队协作，对两个开发者目录，分别设置不同的 `user.name` 信息。

> `git config --local`，使用 `--local` 参数，只对本地参数进行配置。

```
cd leo
git config --local user.name 'leo'

cd jack
git config --local user.name 'jack'
```

### 开发流程
#### master 分支初始化


```
cd leo 
echo a = 1 > leo1.py  // 将a=1写入leo1.py文件
cat leo1.py           //显示文件的内容

git add . 
git commit -m "c4 项目初始化"

git tag 0.0.1  //添加标签 方便管理
```

#### 创建develop分支

```
git checkout -b develop

echo b = 2 > leo1.py
git add .
git commit -m "建立开发分支并提交"

git push origin develop  //推送到远程分支

gitg   // 使用gitg图形化工具查看提交信息
```

#### leo进行开发

```
git checkout -b leo
echo c=3 > leo2.py
git add .
git commit -m "创建leo分支并开发"

//合并当前开发到dev分支
git checkout develop
git merge --no-ff leo  //合并分支 并保留leo分支信息

gitg  //查看提交历史树  gui
```

> `git merge --no-ff leo`，合并分支，并保留`leo`分支的信息。关于参数`--no-ff`的详情，参考 [Ref](https://segmentfault.com/q/1010000002477106)。


![git-team-usage](https://image-bed-20181207-1257458714.cos.ap-shanghai.myqcloud.com/programming-2019/git-team-usage-1.png)

最后，将开发分支推送到远程。

```
git push origin develop

gitg
```


![git-team-usage](https://image-bed-20181207-1257458714.cos.ap-shanghai.myqcloud.com/programming-2019/git-team-usage-2.png)


#### jack进行开发

首先，从远程拉取最新代码。

```
git pull origin 
```

之后，jack进行日常开发，流程同leo开发，此处不再赘述。


#### release分支测试
项目进展到需发布时，从`develop`分支创建`release`分支，用于测试。测试通过后，将`release`分支合并到`develop`分支和`master`分支上。


## 远程多分支代码拉取

此处记录如何拉取远程所有分支，并建立本地分支追踪远程分支。

* Step 1： `clone` 远程代码
* Step 2： 在sourcetree中可以看出，远程分支有`origin/master`，`origin/dev`和`origin/webtest`。此时本地分支为`dev`，并建立了追踪关系。

![git pull branches](https://image-bed-20181207-1257458714.cos.ap-shanghai.myqcloud.com/programming-2019/git-pull-branches-1.png)

* Step 3： 在对应的远程分支（`origin/master`）上点击，检出分支，设定本地分支名，并追踪远程分支即可。


![git pull branches](https://image-bed-20181207-1257458714.cos.ap-shanghai.myqcloud.com/programming-2019/git-pull-branches-2.png)

* Step 4： 最终效果如下图，检出本地分支并追踪远程。

![git pull branches](https://image-bed-20181207-1257458714.cos.ap-shanghai.myqcloud.com/programming-2019/git-pull-branches-3.png)



> Tip
> 
> `git branch -r`： 查看远程分支
>
> `git branch -a`： 查看所有分支
>
> Ref: [Blog](https://gaohaoyang.github.io/2016/07/07/git-clone-not-master-branch/)



## 多SSH配置
1. 取消全局账户和邮箱设置

```
git config --unset --global user.name
git config --unset --global user.email
```

2. 新建SSH key：

```
cd ~/.ssh
ssh-keygen -t rsa -C "liubaoshuai1@jd.com" 
Enter file in which to save the key (/User/lbs/.ssh/id_rsa): id_rsa_jd  # 输入文件名
```



3. 新密钥添加到 SSH Agent

默认只读取 `id_rsa`，为了让 SSH 识别新的私钥，需将其添加到 SSH Agent 中

```
ssh-add ~/.ssh/id_rsa_jd
```

若出现 `Could not open a connection to your authentication agent` 错误，执行以下命令

```
ssh-agent bash
ssh-add ~/.ssh/id_rsa_jd
```


4. 修改 `config` 文件

```
vim config      # 若没有，可创建  touch config
```


```
# default user (lbs1203940926@163.com)
Host github.com
   HostName github.com
   User lbs1203940926@163.com
   PreferredAuthentications publickey
   IdentityFile /Users/lbs/.ssh/id_rsa


# jd-gitlab(liubaoshuai1@jd.com))
Host git.jd.com
   HostName git.jd.comm
   User liubaoshuai1@jd.com
   PreferredAuthentications publickey
   IdentityFile /Users/lbs/.ssh/id_rs_jd
```


5. 拉取JD代码

```
# 测试暂不支持SourceTree中远程拉取，需要终端命令行拉取
# 且暂只支持https协议

git clone http://git.jd.com/jdreact/jdreact-jsbundle-jdreactonehourarrive

# UserName: liubaoshuai
# PassWord: ERP's password
```