---
title: 使用hexo创建个人Blog
tags:
  - hexo
  - blog
categories:
- Hexo
abbrlink: d654e909
date: 2022-02-11 01:10:24
---

## 工具

- node.js   [Node.js 中文网 (nodejs.cn)](http://nodejs.cn/)

- git  [Git (git-scm.com)](https://git-scm.com/)

输入 `node -v` 和 `git --version` 查看是否安装完成

## 使用 hexo 创建网页

`npm install -g hexo-cli ` 命令安装hexo

`hexo init name` 初始化hexo项目目录

在项目目录下使用  `npm install `  安装依赖模块

`hexo g`  生成网站的静态文件

使用 `hexo s` 在本地运行

## 配置网页内容

在 [主题 | Hexo](https://hexo.io/zh-cn/docs/themes.html)  或 github 找个适合的(最好是中文，不然看不懂😂)主题 git clone 到主题文件夹下

通过hexo目录下的站点配置文件`_config.yml` 选择刚下载的主题并修改一些基础信息

修改主题文件夹下的主题配置文件 `_config.yml` 文件编辑网页的样式

通过 `hexo new "name"` 生成md文件(blog文章) 

### 我的next主题配置

- 语言：站点配置文件: language:**zh-CN**

- 菜单：主题配置文件：

  ```yml
  menu:
    home: / || fa fa-home
    #about: /about/ || fa fa-user
    tags: /tags/ || fa fa-tags
    categories: /categories/ || fa fa-th
    archives: /archives/ || fa fa-archive
    #schedule: /schedule/ || fa fa-calendar
    #sitemap: /sitemap.xml || fa fa-sitemap
    #commonweal: /404/ || fa fa-heartbeat  
  ```

  ```shell
  hexo new page "categories"
  hexo new page "tags"
  ```

  ```markdown
  ---
  title: 分类
  date: 2022-3-12 22:00:00
  type: "categories"
  ---
  ```

  ```markdown
  ---
  title: 标签
  date: 2022-3-12 22:00:00
  type: "tags"
  ---
  ```

- 主题样式：主题配置文件

  ```yml
  # Schemes
  # scheme: Muse
  scheme: Mist
  # scheme: Pisces
  # scheme: Gemini
  
  # Dark Mode
  darkmode: false  //黑暗模式
  ```

- 进度显示: 主题配置文件

  ```yml
  back2top:
    enable: true
    # Back to top in sidebar.
    sidebar: true    //开启侧边栏显示进度
    # Scroll percent label in b2t button.
    scrollpercent: true	
  ```

- 代码块复制:主题配置文件

  ```yml
  back2top:
    enable: true
    # Back to top in sidebar.
    sidebar: true    //开启侧边栏显示进度
    # Scroll percent label in b2t button.
    scrollpercent: true	
  ```

- 本地搜索：主题配置文件 

  ```shell
  npm install hexo-generator-searchdb --save
  ```

  ```yml
  local_search:
    enable: true
    # If auto, trigger search by changing input.
    # If manual, trigger search by pressing enter key or search button.
    trigger: auto
    # Show top n results per article, show all results by setting to -1
    top_n_per_article: 1
    # Unescape html strings to the readable one.
    unescape: false
    # Preload the search data when the page loads.
    preload: false
    #my settings
  ```

- 图标:主题配置文件 

  ```yml
  favicon:
    small: /images/favicon-16x16-next.png
    medium: ../../../assets/icon.png
    apple_touch_icon: /images/apple-touch-icon-next.png
    safari_pinned_tab: /images/logo.svg
  ```

- 阅读时长:主题配置文件 

  ```shell
  $ npm install hexo-symbols-count-time
  ```

  ```yml
  symbols_count_time:
    separated_meta: true
    item_text_post: true
    item_text_total: false
  ```

- live2d:主题配置文件 or 站点配置文件

  ```shell
  npm install -save hexo-helper-live2d
  npm install --save live2d-widget-model-hijiki
  ```

  ```yml
  live2d:
    enable: true
    scriptFrom: local
    pluginRootPath: live2dw/
    pluginJsPath: lib/
    pluginModelPath: assets/
    tagMode: false
    log: false
    model:
      use: live2d-widget-model-<你喜欢的模型名字>
    display:
      position: right
      width: 150
      height: 300
    mobile:
      show: true
  ```

- 文章链接 :站点配置文件

  ```shell
  npm install hexo-abbrlink --save
  ```

  ```yml
  permalink: posts/:abbrlink/
  abbrlink:
  	alg: crc32   #算法： crc16(default) and crc32
  	rep: hex     #进制： dec(default) and hex
  ```

## 使用github 发布blog

在github上创建仓库名为`用户名.github.io`的`公开`仓库（不公开不行，后期也不能关，关了在打开也不行）

使用 `npm install hexo -deployer-git --save`命令安装deployer模块

打开的Hexo项目文件夹下的`_config.yml`文件找到 `deploy:` 添加

```
type: git
repo: https://github.com/你的用户名/你的用户名.github.io.git
branch: master
```

回到hexo目录

```
hexo clean           #清除缓存      可缩写hexo c
hexo generate        #生成静态文件   可缩写hexo g
hexo deploy        	 #部署到github  可缩写hexo d
#再次
hexo d				 #部署到github远程仓库 
```

地址栏输入github用户名.github.io即可看到blog了
