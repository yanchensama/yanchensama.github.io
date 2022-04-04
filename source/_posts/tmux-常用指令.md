---
title: tmux 常用指令
tags:
  - tmux
  - linux
categories:
  - Linux
abbrlink: 24a9fddd
date: 2022-04-03 23:48:17
---

>tmux全称Terminal MultipleXer：终端复用器;
>
>tmux 操作需要前缀键，默认是ctrl+b , 改成ctrl +a更好按一些

- tmux  可以有多个session，每个会话有多个window，每个window有多个pane

  - session

    - window

      - pane
      - pane

    - window

      ......

  - session

    ......

    ![image-20220404003853835](../assets/img/tmux-常用指令/image-20220404003853835.png)

    从外到内分别是3个session，3个window，3个pane
    
    tip：没啥用，一般就开一个会话，一个window，多分几个pane就完事（屏幕大就不介意）

- tmux 或 tmux new -s <session_name>：开启新session（取名完全没必要，默认0，1，2.。。）

- tmux  a 或 tmux a -t <session_name>： 连接已存在的session

- ctrl + a s 或 tmux ls:  查看session

- ctrl +a w:  查看window

- 睁开眼睛 ：查看pane

- 切换session和window 就 crrl+a s 或者ctrl +a w 查看，然后选就好了

- ctrl +a  c：创建新window

- ctrl + a % ：水平拆分pane

- ctrl + a  " ：垂直拆分pane

- ctrl + a  方向键：切换pane

- ctrl + a  z：开启/取消 当前pane全屏

-  ctrl+ a  x：关闭光标所在pane

- ctrl + a & ：关闭 window

- ctrl+a :kill-session ：关闭session

- ctrl+a: kill-server : 全关

- ctrl +a  d :  挂起

