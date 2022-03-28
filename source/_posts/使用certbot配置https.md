---
title: 使用certbot配置https
abbrlink: d3ca7562
date: 2022-03-14 15:26:03
tags:
---

>

> 官网https://certbot.eff.org/

## 安装snap

- apt和snap

[ubuntu](https://so.csdn.net/so/search?q=ubuntu&spm=1001.2101.3001.7020)用apt代替了apt-get，ubuntu的母公司Canonical在最新版的Ubuntu中用snap代替了apt。
类似于[yum](https://so.csdn.net/so/search?q=yum&spm=1001.2101.3001.7020)代替了rpm，而后又使用dnf代替了yum。

- centos8 添加EPEL库（其它版本参考https://snapcraft.io/docs/installing-snapd/）

[Extra Packages for Enterprise Linux](https://fedoraproject.org/wiki/EPEL) (EPEL)

```shell
sudo dnf install epel-release
sudo dnf upgrade
```

- 安装

```shell
sudo systemctl enable --now snapd.socket
```

```bash
sudo systemctl enable --now snapd.socket
```

