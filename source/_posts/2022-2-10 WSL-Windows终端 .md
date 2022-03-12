---
title: WSL-Windows终端
tags:
  - wsl
  - linux
  - ubuntu
  - terminal
categories:
  - Linux
abbrlink: b78424a8
date: 2022-02-10 13:55:45
---
# WSL-Windows终端

## 介绍

- `CMD`

  经典又丑陋的小黑框

- `Windows Terminal`

  基于 .Net Framework 开发的替代cmd的工具，增强版cmd

- `Windows Terminal`

  PowerShell、CMD  、WSL 三大环境实现了统一的终端

## 美化

- ### 透明化

打开设置文件settings.json,找到这段代码

```
"defaults":
        {
            // Put settings here that you want to apply to all profiles.
        },
```

添加以下代码

```
"useAcrylic": true, //毛玻璃特效的开关
"acrylicOpacity": 0.7 //透明度的设置，设置为“0”是全透明，“1”是不透明
```

- ### 图标

在对应终端的中括号内添加，如下

```
 {
                "guid": "{2c4de342-38b7-51cf-b940-2309a097f518}",
                "hidden": false,
                "name": "Ubuntu",
                "source": "Windows.Terminal.Wsl",
                "icon": "图标地址"
            }
```

## 安装oh-my-zsh(Ubuntu)

安装并切换到zsh

```
sudo apt-get install zsh
chsh -s /bin/zsh 
```

重新打开就会发现已经是zsh的样子了

然后安装oh-my-zsh

下面的是自动安装脚本

```
sh -c "$(wget https://raw.github.com/robbyrussell/oh-my-zsh/master/tools/install.sh -O -)"
```

不行很大可能是github被墙了

试试手动安装

```
git clone https://github.com/skillf-qf/ohmyzsh.git
cd ./ohmyzsh/tools/
./install.sh
```

然后是添加插件，我一般开三个，一个语法高亮，一个命令提示，一个自带的补全sudo

```
cd $HOME/.oh-my-zsh/plugins
#下载代码
git clone https://github.com/zsh-users/zsh-syntax-highlighting.git
#自动配置
echo "source ${(q-)PWD}/zsh-syntax-highlighting/zsh-syntax-highlighting.zsh" >> ${ZDOTDIR:-$HOME}/.zshrc
```

```
cd $HOME/.oh-my-zsh/plugins
#下载代码
git clone https://github.com/zsh-users/zsh-autosuggestions ${ZSH_CUSTOM:-~/.oh-my-zsh/custom}/plugins/zsh-autosuggestions
```

编辑~/.zshrc在plugin（git）中添加 zsh-autosuggestions ，sudo

添加history时间

```
code ~/.zshrc
#HIST_STAMPS="yyyy-mm-dd"取消注释
```

最后

```
source ~/.zshrc
```

至此完成zsh配置

