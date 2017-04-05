---
layout: title
title: git学习记录
date: 2017-03-17 17:09:02
categories: Font-end
---

公司一直用的是**svn**（集中式版本控制系统 ），一般日常提交代码用**update**（从中央服务器上获取最新代码）和**commit**（将本地代码提交上去，提交前会用*Beyond Compare*软件对比修改一下）两个命令就能满足一般需求，偶尔会看一下日志，用一下版本回滚。但是最近搭博客，开始使用github，了解到**git**（分布式版本控制系统）好像很火？大家都在推荐，于是用了一下这个新工具。

首先，如果你想详细了解git原理和使用，狠狠点击[这里](https://git-scm.com/book/en/v2)！此外，还有很多关于git不同风格的博客啊，比如[廖雪峰](http://www.liaoxuefeng.com)写的git教程。
但是，如果你口味清奇有趣，可以选择[这个](http://learngitbranching.js.org/)，敲生动，适合食用！

然后，列一下学习记录：

先学点**简单**的：

1.网上下载安装git，得到大礼包：**Git Bash**，**Git GUI**，**Git CMD**，一般操作使用**Git Bash**即可。
2.本地新建文件，使用*git init*将它变成待用仓库。

<!--more-->

3.获取代码：

``` dos
	git clone <server url> 			//将远程仓库 上的代码拉到仓库中。
``` 

4.修改代码后，这样提交它：

``` dos
	git add <filename>               //提交 到缓存区（index） 
``` 

或者使用：

``` dos
	git add  .                  	//提交 所有改动文件到缓存区
``` 

再将其提交到本地仓库中：

``` dos
    git commit -m "修改信息"     	//提交到本地仓库中的HEAD上
``` 

期间，你可以用*git status*查看文件变化。

下图是我修改blog中的文件时，*git status*返回的文件状态
<img src="../../../../assets/img/3-17-1.png"   align=center />

``` dos
	git push origin master          //提交到远程仓库origin
``` 
 
<img src="../../../../assets/img/3-17-2.png"   align=center />

如果你没有进行第3步，这里使用：

``` dos
	git remote add origin <server url>			//提交到远程仓库
``` 

5.我们经常要将远程仓库中的代码更新本地：

``` dos
	git pull
``` 

满足基本的需求后，我们了解一下git的分支功能：

1.创建、查看、切换分支：

``` dos
	git branch newbranch               //新建一个名newbranch的分支
    git checkout -b newbranch     		//新建一个名newbranch的分支并切换到新分支
    git branch                         //查看分支
    git checkout master                 //回到主分支master
``` 
   
然后就可以在确定的分支下操作（比如上方的基础操作）。

2.进行分支间的操作，比如：

``` dos
	 git branch -d newbranch        //删除这个分支  
     git merge <branchname>         //合并分支到当前分支
``` 

来一张learngitbranch的图：
<img src="../../../../assets/img/3-17-3.png"   align=center />

此时可能产生冲突，用*git status*查看情况，再去文件修改解决冲突，用*git add <filename>*将冲突的文件标记为解决，再用*git status*确认冲突被解决，然后我们就可以用*git commit*提交了。

当然除了分支，git还有很多功能（标签，撤销操作，版本回退），当有其他情况可以去查找命令，以上。 

