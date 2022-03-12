---
title: linux下/opt和/usr/local目录的区别
tags:
  - linux
  - tips
categories:
- Linux
- Note
abbrlink: 6acffbaa
date: 2022-02-14 22:05:44
---

# Linux 程序安装目录 /opt 和 /usr/local 的区别

/usr/local 目录和 /opt 目录都是用来安装第三方软件的目录，所谓第三方软件其实就是用户自己安装的软件，区别于安装系统时自带的软件。

- /usr/local 下一般是你安装软件的目录，这个目录就相当于在windows下的programefiles这个目录；
- /opt 这个目录是一些大型软件的安装目录，或者是一些服务程序的安装目录。

/usr/local和/opt不同的是，/usr/local命令下面的一些子目录往往都是被加入到PATH环境变量中的（PATH中默认就有/usr/local/bin，可以使用echo $PATH查看），而/opt目录则没有在PATH环境变量中，这样安装在/usr/local目录下的软件就可以在命令行执行、启动。

但是这也不是绝对的，也可以把需要命令启动的软件安装在/opt目录，然后在/usr/local/bin目录建立一个链接文件，这样同样可以命令启动这个软件，网上许多Linux软件安装教程都会采用这个方法。
