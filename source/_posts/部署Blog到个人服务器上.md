---
title: 部署Blog到个人服务器上
tags:
  - linux
  - blog
  - nginx
  - git
categories:
  - Hexo
  - Git
  - Linux
abbrlink: fbf5147f
date: 2022-03-12 22:11:06
---
##  vscode远程连接服务器

vscode安装 **remote-ssh** ，ctrl+shift+p 呼出设置，选择add new host，添加服务器信息,

选择你的服务，在新窗口输入密码即可

## 创建个人git仓库代替时好时坏的Github（为了流畅打开Blog）

blog源码上传到GitHub（毕竟永久免费空间），编译后的代码上传到个人仓库

创建git用户，该用户下目录即为git空间

```
adduser git  #创建用户
passwd git	 #设置密码
su git     	 #切换用户 
cd /home/git  #切换目录
mkdir -p projects/blog   //创建博客的静态文件仓库，主页入口
mkdir repos   #创建git仓库
cd repos
git inti --bare blog.git #初始化一个git“裸仓库”，
cd blog.git/hooks #进入钩子目录（钩子函数，回调函数，类似后置触发器，在特定环节生效）
cp post-update.sample post-update #创建钩子函数（git提交时自动部署）
#内容如下： git提交到/repos/blog.git这个裸仓库后，部署到project/blog这个目录（也就是将html等文件放这里）
#!/bin/sh
git --work-tree=/home/git/projects/blog --git-dir=/home/git/repos/blog.git checkout -f

# 修改权限
chmod +x post-update #执行权限总得有吧
exit // 退出到 root 登录   
chown -R git:git /home/git/repos/blog.git // 将blog.git及子目录拥有者设为git，git用户需要有对blog.git的写入权限

#使用密钥免密登录：这样git提交时可以不用输密码（git用户的密码）
su git     //切换git用户
cd /home/git   //进入git目录
mkdir .ssh  //创建免密公匙目录
cd /home/git/.ssh  //注意是用git用户进这个目录，不是root
touch authorized_keys  //存放客户端的ssh公钥(id_rsa.pub)  
chmod 600 authorized_keys   //配置权限  
cd .. //退出.ssh目录  
chmod 700 .ssh  //.ssh目录必须700权限
#拷贝本地的.ssh/id_rsa.pub （没有的话使用ssh-keygen命令生产）内容到 authorized_keys 内
```

使用git init --bare “repo”可以创建一个裸仓库（.git后缀的仓库），并且这个仓库是可以被正常clone和push的，裸仓库不包含工作区（不可在该仓库执行git操作），所以不能在裸仓库上直接提交变更。裸仓库是作远程仓库使用的。毕竟裸仓库是只作为真正的仓库用的，只用来push和clone的，没必要在这上面添加不需要的工作区（会干净点）。Github上的仓库也都是这样的。

如果不使用“–bare”参数，初始化仓库后，提交master分支时报错。这是由于git默认拒绝了push操作(不能push到有工作区的仓库)，但是可以在.git/config添加如下代码：

```
[receive]
denyCurrentBranch = ignore
```

## nginx设置

创建服务

```
server {
    listen 1234;
    location / {
    root   /home/git/projects/blog;
    index  index.html  index.htm;
    }
}
```

 在阿里云网站里开放tcp端口1234，才能在本地访问1234端口。

### 补充：

hexo-deployer-git 工作流程

hexo-deployer-git 部署的方式是在 hexo 项目根目录下创建了一个 .deploy_git 文件夹, 并在其中创建了一个独立的 git 分支, 将生成静态文件移入这个文件夹中并推送到指定的地址. 但 .deploy_git 文件夹默认也被写入 .gitignore 文件中, hexo 项目 git 库不会记录这个文件夹, 同时 hexo-deployer-git 在每次部署时也不会自动同步服务器上的提交历史, 而是强制覆盖旧的提交. 在新的电脑或路径上重新 clone 后也会出现旧的静态文件记录丢失的情况, 重新 deploy 后服务器上旧的部署历史也会丢失.

如果想要保存每次的部署记录, 那么就可以将 .deploy_git 中的文件也看做一个子项目, 以子项目的形式提交到 hexo 主项目中保存, 就可以保持部署记录不丢失, 并且在任何地方重新 clone 时都可以恢复最新的记录.

添加 .deploy_git 中分支为子项目

.deploy_git 中是由部署插件在 hexo 项目上创建的一个独立分支, 只需通过传递 -b 选项将 hexo 项目的这个分支作为主项目的依赖即可, 例如部署在 coding 时, 使用的 coding-pages 分支:
```
git submodule add -b coding-pages <site>
```