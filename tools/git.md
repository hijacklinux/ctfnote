---
layout: default
---
# ![](../img/hj.jpg)git的使用
>小贱提示：
>
>用atom的git-plus插件很方便，点击右下角file


## ![](../img/github1.png)简单工作流程
#### git add 暂存刚刚更改的
git add 文件名
git add . 	添加所有
#### git status 检查当前文件状态
两种状态：
已跟踪：被纳入版本控制管理的文件，在上次快照中有它们的记录，工作一段时间后，它们的状态可能是：未更新，已修改或已放入暂缓区

未跟踪：除以上的其他文件

查看当前文件状态：
git status
git status -s   #精简方式查看

#### git diff 查看差异
查看未暂存的文件相比于已暂存的文件更新了哪些部分：
git diff

查看已暂存的文件相比于上次提交时的快照之间的差异：
git diff --staged

#### git commit 提交
git commit 即保存仓库中的历史纪录

git commit -m '这里是修改说明'

git commit -a -m '这里是修改说明''  //跳过暂存，直接提交

amend参数仅仅是修改commit注释用的

## ![](../img/github2.png)git reset --hard回到过去
git reset 							#从staged返回到unstaged

git reset --hard HEAD 		#返回到日志里面最新的那个记录（把所有staged返回到unstaged）

回到过去方法1：

git reset --hard HEAD^		#将指针向前移一个日志记录，等价于	git reset --hard HEAD~1

git reset --hard HEAD^^	#将指针向前移两个日志记录，等价于	git reset --hard HEAD~2

回到过去方法2(推荐)：

git reset --hard 2a17846

#2a17846为git log --oneline的short hash
## ![](../img/github3.png)git reflog 回到未来
先用git reflog 查看全局历史（包括未来）

git reset --hard 短哈希值  		#回到未来

也可以

git reset --hard HEAD@{2}  	#也可以这种方式回到未来

## ![](../img/github4.png)git checkout对单个文件回到过去
git checkout 短哈希/HEAD标签 -- 1.py

#回到未来或过去都行，但不影响git log --oneline，因为只改变自己，并不改变周围的朋友

## ![](../img/github5.png)git log 查看提交历史
显示历史（不包括未来）

|           指令            |           说明           |
| ------------------------- | ------------------------ |
| git log                   | 显示所有文件日志         |
| git log --pretty=short    | 只显示1行                |
| git log --oneline         | 更简洁地显示             |
| git log --oneline --graph | 有分支时能用到，图形方式 |
| git log xxx               | 显示指定文件的日志       |
| git log -p                | 显示文件的改动           |


## ![](../img/github6.png)配置git
直接用[python一键配置](https://github.com/hijacklinux/git_configpy)

## ![](../img/github7.png)取得项目的git仓库
#### 方法1：创建新的git仓库
从当前目录初始化（进入到项目的目录下）执行：
```
git init
初始化后会出现一个.git的目录，所有git需要的数据和资源都在这里
用git add 命令告诉git开始对项目目录的哪些文件进行跟踪，然后提交：
git add *.py
git add readme
git commit -m 'initial project version'
```
#### 方法2：从已有的git仓库克隆
git clone url地址 test

这会在当前目录下创建一个名为test的目录，其中内含一个.git的目录，并从同步后的仓库中拉出所有的数据，取出最新版本的文件拷贝


## ![](../img/github8.png)忽略某些文件
建立一个文件名为.gitignore的文件，内容为（一个文件名一行）如：
```
*.[oa]
*.~
*.log
*.tmp
*.pid
tmp/   用/代表文件夹
```

## ![](../img/github9.png)git rm 移除
移除跟踪但不删除文件： git rm --cached test.py

啥都删除包括本地文件：git rm -f test.py
## ![](../img/github10.png)git mv 移动
git mv 原文件 新文件

相当于以下三条命令：
```
mv 原文件 新文件
git rm 原文件
git add 新文件
```

## ![](../img/github11.png)git branch 分支
#### 创建并切换分支
git checkout -b fenzhi1		#创建分支并将当前分支切换为:fenzhi1

相当于下面两条：
```
git branch fenzhi1 				#创建分支
git checkout fenzhi1			#将当前分支切换为:fenzhi1
```
#### 创建相应的远程github分支fenzhi1
git push origin fenzhi1
#### 查看分支列表
git branch
#### 切换到上一个分支
git branch -
#### 删除分支
git branch -d fenzhi1
>小贱提示：
>要从fenzhi1切换出去之后才能删除
#### 合并分支
git merge --no-ff

第一步：切换到master分支			git checkout master

第二步：合并fenzhi1分支				git merge --no-ff fenzhi1 -m 'keep merge info'

--no--ff  		#no fast forward保留原master的备注信息
#### 分支冲突
单选:use

都保留:删除>>>和<<<以及===，保存，双击右边的，staged
## ![](../img/github12.png)临时要修改别的分支，当前还没stage
假如说我现在在弄A分支，老板突然让我搞B分支

先在A上ctrl+s，然后stash save changes

接下来就可以切换到B分支完成临时老板分配的任务,如果需要，也可以把b合并到master

结束后继续自己的工作：stash pop
## ![](../img/github13.png)上传github
#### step 1
登录github帐号，点击 new repository输入项目名称(例如：code)。
以前没有这个git仓库的话，就勾选Initialize this repository with a README，有的话

#### step 2
进入终端在当前目录下创建一个项目mkdir code,然后进入到该目录下 cd code.

#### step 3
检测项目到本地，在新建项目的页面右下角有HTTPS clone URL,复制项目的gitURL到粘贴版，使用git clone将URL粘贴到后面(git clone https://github.com/sugerchestnut/code.git)
将项目克隆到本地服务器。

#### step 4
进入到该项目cd code，
就会看到README.md文件也会被检出在本地环境中，
这样就会有一个本地工作区，
可以在本地工作区进行修改等一系列操作，然后提交。

#### step 5
5>将要上传的文件复制到该项目里，可以一次上传多个文件，
首先使用git status查看工作区状态，
再使用git add +项目里的文件名，
使git跟踪到新增的文件，(随时使用git status命令查看当前工作区状态)。

#### step 6
执行git commit命令执行本次提交的变更，填写变更的评论(目的)，填写完后ctrl+c，ctrl+x退出。
简单点：git commit -m '这里是修改说明‘
git commit -a -m '这里是修改说明‘  //跳过暂存，直接提交

#### step 7
此时工作区已经干净，没有要提交的文件了，
最后使用git push来发布本地的提交操作，
输入github帐号和密码。这样提交就完成了。



__原创文章，转载请注明转载自[http://www.8pwn.com](http://www.8pwn.com)__

[返回上一层](./tools)
