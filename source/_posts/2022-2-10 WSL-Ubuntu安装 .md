---
title: WSL-Ubuntu安装
abbrlink: c1cc855b
date: 2022-02-10 12:55:45
tags:
- wsl
- linux
- ubuntu
categories:
- Linux
---



## 安装wsl-ubuntu

现版本微软提供了一键安装wsl-Ubuntu的命令

```
wsl --install
```

默认就是安装wsl2和ubuntu，选择其他版本参考微软官方文档[安装 WSL | Microsoft Docs](https://docs.microsoft.com/zh-cn/windows/wsl/install)

安装完成后输入用户名密码即可使用

在终端输入以下命令以更新系统软件包

```
sudo apt update
sudo apt upgrade
```

安装neofetch 查看版本等信息

```
sudo apt install neofetch
```

输入`neofetch`以查看
