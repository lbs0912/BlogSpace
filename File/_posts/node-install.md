---
title: Node.js安装和NPM路径修改
date: 2016-09-25 10:06:25
tags: [Node,Front-end]
categories: Front-end
---
本文主要记录了Node.js的安装和NPM路径的修改。
<!-- more -->
## Node.js和NPM下载与安装
Windows系统下，**Node.js**的安装比较简单，可以在其官方网站下载并安装。

**Node.js官网下载** : https://nodejs.org/en/

此处以`Node.js v4.4.7 LTS`版本为例，下载的文件为`.msi`文件，可以直接点击安装。此处将Node.js的安装路径修改为`D:\Program Files\nodejs`。

由于新版Node.js中已经集成了npm，因此，安装Node.js的同时，也进行了npm的安装。

## Node.js和NPM安装验证
Windows系统下，在CMD窗口中，输入`node -v`验证Node.js是否安装成功；输入`npm -v`验证npm是否安装成功。

若安装成功，会显示Node.js和npm对应的版本号，如下图所示。

![Node.js和NPM安装验证](http://oda53d5ps.bkt.clouddn.com/nodejs.PNG)

## NPM路径修改
**NPM**是node.js的一个模块管理工具，其默认的路径在用户文件下的`Appdata\Local`目录下。本文中，npm默认的路径为`C:\Users\lbs\AppData\Local`。

为了方便后期对npm管理，可以将npm的全局管理模块的存放路径和npm cache的存放路径修改为node.js的安装路径。

### node_global和node_cache文件夹创建
在node.js的安装路径（本文中为`D:\Program Files\nodejs`）下，创建`node_global`和`node_cache`文件夹。

### CMD指令操作 
在CMD窗口下分别输入如下指令。

```
npm config set prefix "D:\Program Files\nodejs\node_global"
```

```
npm config set cache "D:\Program Files\nodejs\node_cache"
```

###  npmrc文件修改
修改node.js安装目录下的`node_modules\npm`中的`npmrc`文件。本文中`npmrc`文件的路径为`D:\Program Files\nodejs\node_modules\npm`。

将`npmrc`文件修改为

```
prefix=D:\Program Files\nodejs\node_global
cache=D:\Program Files\nodejs\node_cache
```
### NODE_PATH系统变量设置
进入我的电脑-->属性-->高级-->环境变量,在系统变量下新建"NODE_PATH"，输入"D:\Program Files\nodejs\node_modules"，如下图所示。

![npm系统变量设置](http://oda53d5ps.bkt.clouddn.com/nodepath.PNG)


## 参考资料
1. [windows 下安装nodejs及其配置环境](http://www.cnblogs.com/pigtail/archive/2013/01/08/2850486.html)
2. [修改npm下载模块的安装位置](http://blog.csdn.net/wyc_cs/article/details/51025614)

## 反馈与建议
- 邮箱：<lbs1203940926@163.com>
- 微信：[@脱缰的哈士奇(ab1203940926)](http://ojx8u3g1z.bkt.clouddn.com/wechat-id.jpg)
- 微博：[@脱缰的哈士](http://weibo.com/2329754491/profile) 
