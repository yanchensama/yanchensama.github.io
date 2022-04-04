---
title: git操作
tags:
  - git
categories:
  - Git
abbrlink: 46390b34
date: 2022-03-12 23:03:37
---
## git常用命令

- git init 初始化一个git仓库

- git status 查看仓库状态

- git clone 仓库链接    克隆仓库**版本库**到本地

- git branch --set-upstream-to 仓库名/分支名 设置当前分支的**upstream**分支

  注：设置的**upstream**需要和你的本地仓库分支**对应**，否则设置无效

- git push  将你的版本库推送至你的**upstream**分支进行合并，没有设置会提醒你设置

- git pull   从你的**upstream**分支版本库**拉取到本地并合并**，没有设置会提醒你设置

  注：以下两操作虽然可以跨分支操作，还是要填写与其对应的分支名，请不要作死！！

- git push 仓库名 分支名  未设置upstream时，推送版本库至仓库分支进行合并

- git pull 仓库名 分支名   未设置upstream时，拉取版本库至仓库分支进行合并

- git pull ≈ git fetch + git merge   后者可以微操，但是我不会，那没事了

- git add . 添加到暂存区

- git commit -m "备注"   提交到版本库  

- git diff  xx 暂存区不为空时，将工作其文件与暂存区进行比较，暂存区为空时，与上个版本比较

- git restore xx  暂存区不为空时，回退到暂存区，暂存区为空时，回退到上个版本

- git restore --staged xx 将文件从暂存区移出

- git reset  --hard 版本号 回退版本

- git log （--pretty=oneline）版本日志

- git reflog            HEAD指针移动日志（包括回退的记录）

- git remote add  添加远程仓库

- git remote rm   删除远程仓库

  #### 分支操作

  - git branch 查看分支

  - git checkout -b 分支名  创建并切换至新分支

  - git branch -d 分支名 删除分支

  - git checkout 分支名  切换分支

  - git merge 分支名 合并分支

  - git push -d 远程仓库名 分支名 删除远程分支

  - git branch --set-upstream-to=仓库名/分支名 本地分支名  将本地分支连接到云端分支

    

## git子模块（即一个git仓库内有其它git仓库存在）

-  添加子模块
```
git submodule add <url> <path> 
# 该命令会从头clone一份子模块
#url为子模块的路径，path为该子模块存储的目录相对路径
```
该命令生成如下.gitmodules文件，如果想设置的子模块已在本地并不想重新在clone一次，也可以直接编辑该文件，把path设置你的子模块路径就好
```
[submodule "themes/pure"]
	path = themes/pure
	url = https://github.com/cofess/hexo-theme-pure
```
```
git add . 
git commit -m “”
```
- 子模块使用
```
git submodule init 
git submodule update
```
- 删除子模块
```
rm -rf #子模块目录 删除子模块目录及源码
vi .gitmodules #删除项目目录下.gitmodules文件中子模块相关条目
vi .git/config #删除配置项中子模块相关条目
rm .git/module/* #删除模块下的子模块目录，每个子模块对应一个目录，注意只删除对应的子模块目录即可
#执行完成后，再执行添加子模块命令即可，如果仍然报错，执行如下：
git rm --cached 子模块名称
```
- note：
    如果想保存自己修改过的子模块，如博客主题，可以先fork你中意的主题仓库，然后用自己的fork的仓库地址作为子模块url

