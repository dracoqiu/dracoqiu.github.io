---
title: Hexo博客迁移
date: 2018-05-04 22:36:10
categories: "Hexo教程"
tags:
    - Hexo
---

我们会需要在其他电脑继续写博客，所以就在github上创建个分支来存放Hexo的数据。
<!-- more -->
具体的思路是：在生成的已经推到github上的hexo静态代码出建立一个分支，利用这个分支来管理自己hexo的源文件。
具体步骤为：

### 克隆gitHub上面生成的静态文件到本地

```
git clone https://github.com/yourname/hexo-test.github.io.git
```

把克隆到本地的文件除了git的文件都删掉，找不到git的文件的话就到删了吧。不要用`hexo init`初始化。
将之前使用hexo写博客时候的整个目录（所有文件）搬过来。把该忽略的文件忽略了
**建议使用以下的方式并为你的电脑配置SSH，这样推送的话就不需要每次输入帐号密码！**

```
git clone git@github.com:yourname/hexo-test.github.io.git
```

### 创建一个叫hexo的分支

```
git checkout -b hexo
```

### 提交复制过来的文件到暂存区

```
git add --all
```

### 提交

```
git commit -m "新建分支源文件"
```

### 推送分支到github

```
git push --set-upstream origin hexo
```

到这里基本上就搞定了，以后再推就可以直接git push了，hexo的操作跟以前一样。如果使用的是自定义主题，留意查看是否上传到github了。
今后无论什么时候想要在其他电脑上面用hexo写博客，就直接把创建的分支克隆下来，npm install安装依赖之后就可以用了。

### 克隆分支的操作

```
git clone -b hexo https://github.com/yourname/hexo-test.github.io.git
```

因为上面创建的是一个名字叫hexo的分支，所以这里`-b`后面的是hexo，再把后面的gitHub的地址换成你自己的hexo博客的地址就可以了。
这样做完了以后，每次写完博客发布之后不要忘了还要`git push`把源文件推到分支上。
文章转载于 [`https://www.jianshu.com/p/beb8d611340a`](https://www.jianshu.com/p/beb8d611340a)