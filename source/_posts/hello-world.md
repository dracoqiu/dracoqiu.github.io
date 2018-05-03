---
title: github + hexo 搭建个人博客
categories: "Hexo教程" #文章分類目錄 可以省略
tags:
     - Hexo教程
description: #你對本頁的描述 可以省略
---
GitHub Pages 是通过我们网站托管和发布的公开网页。所以利用这个可以搭建个人博客。
<!--more-->
# 如何搭建个人博客
## GitHub配置
### 注册GitHub账号
前往[GitHub](https://github.com/)注册账号，并且创建一个New repository。Repository name填写为dracoqiu.github.io，dracoqiu这个为用户名，自行修改。创建完成后在项目的Settings里找到GitHub Pages，会发现这里有显示你的博客地址。
### 配置SSH-Key
给电脑配置SSH-Key的话就不用每次都输入账号密码。
1、打开git的命令行工具输入以下命令
```bash
$ ssh-keygen -t rsa -C "your e-mail"
```
创建成功后会看到以下内容
Your identification has been saved in /c/Users/XZY-06/.ssh/id_rsa.
Your public key has been saved in /c/Users/XZY-06/.ssh/id_rsa.pub.
打开文件夹，然后找到id_rsa.pub，用编辑器打开复制里面的内容，然后登陆你的GitHub账户，依次点击账号Settings > SSH and GPG keys > new SSH key，把id_rsa.
pub中的内容拷贝进去key项，title相当于备注，可随意填写 。

## 安装Hexo
### 什么是 Hexo？
Hexo 是一个快速、简洁且高效的博客框架。Hexo 使用 [Markdown](https://daringfireball.net/projects/markdown/)（或其他渲染引擎）解析文章，在几秒内，即可利用靓丽的主题生成静态网页。
### 安装前提
+ [Node.js](https://nodejs.org/en/)
+ [Git](https://git-scm.com/)

### 安装 Hexo
```bash
$ npm install -g hexo-cli
```
安装 Hexo 完成后，请执行下列命令，Hexo 将会在指定文件夹中新建所需要的文件。
```bash
$ hexo init <folder>  # 新建一个网站。如果没有设置 folder ，Hexo 默认在目前的文件夹建立网站。
$ cd <folder>
$ npm install
```
新建完成后，指定文件夹的目录如下：
```bash
.
├── _config.yml  # 网站的 配置 信息，您可以在此配置大部分的参数。
├── package.json  # 应用程序的信息。EJS, Stylus 和 Markdown renderer 已默认安装，您可以自由移除。
├── scaffolds  # 模版 文件夹。当您新建文章时，Hexo 会根据 scaffold 来建立文件。
├── source  # 资源文件夹是存放用户资源的地方。
|   ├── _drafts
|   └── _posts  # md文件存放目录
└── themes  # 主题 文件夹。Hexo 会根据主题来生成静态页面。
```
### 本地预览
运行以下命令将生成静态文件。
```bash
$ hexo generate
$ hexo g  # 简写
```
启动服务器。默认情况下，访问网址为： http://localhost:4000/。
```bash
$ hexo server
$ hexo s  # 简写
```
如果端口被占用运行以下命令给该端口再启动服务器。
```bash
$ hexo server -p 3000
```
在浏览器中输入 localhost:3000就可以看到博客。
### 主题设置
#### 下载主题
打开Git bash，在当前项目根目下使用git从github上checkout主题的代码，输入指令：
```bash
$ git clone https://github.com/iissnan/hexo-theme-next themes/next
```
下载完成后，在hexo\theme目录下回多出一个next文件夹，里面就是next主题所需的文件,当然我们也可以看到在theme文件目录还有一个landscape文件夹，这也就是hexo默认的主题。
#### 配置主题
之前我们配置hexo的时候，有用到`_config.yml`文件，称其为*站点配置文件*，而我们打开next主题文件夹，发现里面也有一个`_config.yml`文件，我们称这个为*主题配置文件*。在hexo中启用next主题的方式：就是打开站点配置文件，找到theme字段，将其值改为“next”，如下：
```bash
# Extensions
## Plugins: https://hexo.io/plugins/
## Themes: https://hexo.io/themes/
theme:  next
```
#### next的样式选择
next的样式其实有三种：Muse、Mist和Pisces，步骤2中看到的其实是next默认的模式Muse，根据官方说明三个样式的特点如下：
*Muse*： 默认 Scheme，这是 NexT 最初的版本，黑白主调，大量留白
*Mist*：  Muse 的紧凑版本，整洁有序的单栏外观
*Pisces*： 双栏 Scheme，小家碧玉似的清新
切换的控制其实很简单，使用next主题配置文件中的`scheme`字段来控制，假设我们选择Mist样式（个人认为最好看的样式），操作步骤是：打开next文件夹中的`_config.yml`文件，找到`scheme`字段，将其设置为“Mist”，如下所示：
```bash
# ---------------------------------------------------------------
# Scheme Settings
# ---------------------------------------------------------------

# Schemes
#scheme: Muse
scheme: Mist
#scheme: Pisces
#scheme: Gemini
```
重新启动博客，刷新浏览器可以看到效果
#### 设置favicon
打开主题配置文件`_config.yml`可以看到favicon的配置信息：
```bash
favicon:
  small: /images/favicon-16x16-next.png
  medium: /images/favicon-32x32-next.png
  apple_touch_icon: /images/apple-touch-icon-next.png
  safari_pinned_tab: /images/logo.svg
  #android_manifest: /images/manifest.json
  #ms_browserconfig: /images/browserconfig.xml
```
#### 菜单栏控制
我们看到页面顶部的菜单栏，其实是由主题配置文件中的menu字段控制的
```bash
menu:
  home: / || home
  about: /about/ || user
  tags: /tags/ || tags
  categories: /categories/ || th
  archives: /archives/ || archive
  # schedule: /schedule/ || calendar
  # sitemap: /sitemap.xml || sitemap
  # commonweal: /404/ || heartbeat
```
然而，点击打开About却出现了“Cannot GET /about/”的页面错误，这是因为我们还没有about这个页面，需要使用`hexo new page "页面名称"`进行创建
```bash
$ hexo new page about
```
执行结果就是在hexo\source目录下面多出了一个about文件夹，里面有index.md，这就是点击About会展示的内容页面。同理，也可以创建tags页面
#### 创建分类页面
添加一个 分类 页面，并在菜单中显示页面链接。
1、新建一个页面，命名为 categories 。命令如下
```bash
hexo new page categories
```
2、编辑刚新建的页面，将页面的类型设置为 categories ，主题将自动为这个页面显示所有分类。注意：如果有启用多说 或者 Disqus 评论，默认页面也会带有评论。需要关闭的话，请添加字段`comments`并将值设置为`false`
```bash
---
title:
date: 2018-05-01 14:25:00
type: "tags"
comments: false
---
```
3、在页面头部可以设置分类以及标签
```bash
---
title: github + hexo 搭建个人博客
categories: "Hexo教程" #文章分類目錄 可以省略
tags:
     - Hexo教程
description: #你對本頁的描述 可以省略
---
```
#### 菜单栏控制
在站点配置文件中假如如下内容，明确指定使用的语言，例如中文
```bash
language: zh-Hans
```
#### 侧栏设置
在主题配置文件的`sidebar`字段，此处我直接设置为侧栏一直显示，而且显示在右边
```bash
sidebar:
  # Sidebar Position, available value: left | right
  position: left
  #position: right

  # Sidebar Display, available value:
  #  - post    expand on posts automatically. Default.
  #  - always  expand for all pages automatically
  #  - hide    expand only when click on the sidebar toggle icon.
  #  - remove  Totally remove sidebar including sidebar toggler.
  #display: post
  display: always
  #display: hide
  #display: remove
```
#### 设置头像和作者名称
在站点配置文件中，新加一个字段`avatar`，值就是头像的连接地址，这里我使用站内地址，将avatar.png放到本地目录hexo\source\images中；作者名称直接设置站点配置文件中`author`字段的值：
```bash
# Site
title: 静然之旅
subtitle:
description:
author: 静然
avatar: /images/avatar.png
language: zh-Hans
timezone:
```
#### 给文章设置阅读量，启用不蒜子统计，仅限于文章页面显示阅读书，在首页不显示。
在主题的配置文件找到`busuanzi_count`字段，把`enable`设置为`true`
```bash
# Show PV/UV of the website/page with busuanzi.
# Get more information on http://ibruce.info/2015/04/04/busuanzi/
busuanzi_count:
  # count values only if the other configs are false
  enable: true
  # custom uv span for the whole site
  site_uv: true
  site_uv_header: <i class="fa fa-user"></i>
  site_uv_footer:
  # custom pv span for the whole site
  site_pv: true
  site_pv_header: <i class="fa fa-eye"></i>
  site_pv_footer:
  # custom pv span for one page only
  page_pv: true
  page_pv_header: <i class="fa fa-file-o"></i>
  page_pv_footer:
```
### 上传到GitHub
打开站点`配置文件`(即更目录下的`_config.yml`)，在最后修改如下代码
```bash
# Deployment
## Docs: https://hexo.io/docs/deployment.html
deploy:
  type: git
  repository: https://github.com/dracoqiu/dracoqiu.github.io.git  # 库（Repository）地址
  branch: master  # 分支名称。如果您使用的是 GitHub 或 GitCafe 的话，程序会尝试自动检测。
  message: [message]  # 自定义提交信息 (默认为 Site updated: {{ now('YYYY-MM-DD HH:mm:ss') }})
```
每次修改本地文件都需执行以下命令
```bash
$ hexo generate
$ hexo deploy
# 以下为简写
$ hexo g
$ hexo d
```
如果执行`$ hexo deploy`提示`ERROR Deployer not found: git`，则执行以下命令，再执行`$ hexo deploy`上传到GitHub
```bash
$ npm install hexo-deployer-git --save
```
### 博客安装和主题配置参考
[hexo官方文档](https://hexo.io/zh-cn/docs/)
[next主题官网](http://theme-next.iissnan.com/getting-started.html#top)
[hexo的next主题个性化配置教程](http://shenzekun.cn/hexo%E7%9A%84next%E4%B8%BB%E9%A2%98%E4%B8%AA%E6%80%A7%E5%8C%96%E9%85%8D%E7%BD%AE%E6%95%99%E7%A8%8B.html)