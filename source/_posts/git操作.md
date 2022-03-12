---
title: git操作
abbrlink: 46390b34
date: 2022-03-12 23:03:37
tags: 
- git
categorie: 
- Git
---
## git子模块（即一个git仓库内有其它git仓库存在）
-  添加子模块
```
# 该命令会从头clone一份子模块
#url为子模块的路径，path为该子模块存储的目录相对路径
git submodule add <url> <path> 
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


