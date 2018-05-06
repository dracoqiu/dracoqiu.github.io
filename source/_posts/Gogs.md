---
title: Gogs
date: 2018-05-06 16:51:16
categories: "杂项"
tags:
    - Gogs
---
Gogs是一款极易搭建的自助 Git 服务。
<!-- more -->
### Gogs有什么优势呢？
- 易安装
您除了可以根据操作系统平台下载 二进制运行，还可以通过 Docker 或 Vagrant，以及 包管理 安装。
- 跨平台
任何 Go 语言 支持的平台都可以运行 Gogs，包括 Windows、Mac、Linux 以及 ARM。
- 轻量级
一个廉价的树莓派的配置足以满足 Gogs 的最低系统硬件要求。有些用户甚至还将 Gogs 运行在 NAS 设备上。

### Gogs安装
安装Gogs可以用`包管理安装`、`二进制安装`和`源码安装`，以下介绍二进制安装，其他安装方式可以看[`官网教程`](https://gogs.io/docs/installation)

#### 选择下载包
根据自己服务器的版本选择合适的下载包，[点击查看更多下载包](https://gogs.io/docs/installation/install_from_binary)

```
wget http://7d9nal.com2.z0.glb.qiniucdn.com/gogs_0.11.43_linux_amd64.tar.gz
```

#### 如何使用下载好的压缩包？
1. 解压压缩包。
```
tar -zxvf gogs_0.11.43_linux_amd64.tar.gz
```
2. 使用命令 cd 进入到刚刚创建的目录。
```
cd gogs
```

3. 执行数据库`SQL`语句
`Gogs`目录下的`scripts/mysql.sql`为使用`MySQL`数据库时需要执行的SQL命令,执行`mysql -u root -p < scripts/mysql.sql`(需要输入数据库密码)初始化数据库

4. 创建数据库gogs用户
创建gogs用户并赋予其gogs数据库的全部权限
```
$ mysql -u root -p
> # （输入密码）
> create user 'gogs'@'localhost' identified by '密码';
> grant all privileges on gogs.* to 'gogs'@'localhost';
> flush privileges;
```

4. 执行命令启动gogs
进入`gogs`的根目录，只需要执行`./gogs web`命令即可启动`gogs`服务，出现一下信息即启动服务成功。
```
2018/05/06 17:08:03 [ WARN] Custom config '/home/download/gogs/custom/conf/app.ini' not found, ignore this if you're running first time
2018/05/06 17:08:03 [TRACE] Custom path: /home/download/gogs/custom
2018/05/06 17:08:03 [TRACE] Log path: /home/download/gogs/log
2018/05/06 17:08:03 [TRACE] Log Mode: Console (Trace)
2018/05/06 17:08:03 [ INFO] Gogs 0.11.43.0330
2018/05/06 17:08:03 [ INFO] Cache Service Enabled
2018/05/06 17:08:03 [ INFO] Session Service Enabled
2018/05/06 17:08:03 [ INFO] SQLite3 Supported
2018/05/06 17:08:03 [ INFO] Run Mode: Development
2018/05/06 17:08:03 [ INFO] Listen: http://0.0.0.0:3000
```
**但不建议你这样做，因为这样是以root用户运行git，最好的是用一个用户专门管理git**
> 建议切换到git用户执行此命令
> su - git
如果提示`su: user git does not exist`，说明没有这个用户，使用`useradd git`创建用户，然后再运行`su - git`，再执行`./gogs web`启动gogs
如果没问题的话通过浏览器访问`http://ip:3000/`就可以看到配置页面，因为首次运行安装程序需要配置相应的数据。

### 使用域名访问gogs
1. 把gogs移动到web服务器根目录
```
mv gogs /home/wwwroot/
```
1. 先配置个域名，如`gogstest.xxxxx.top`
2. 在服务器添加域名配置文件，配置如下
```
server
{
    listen 80;
    #listen [::]:80;
    server_name gogstest.xxxxx.top ;
    index index.html index.htm index.php default.html default.htm default.php;
    root  /home/wwwroot/gogs/public;

    include other.conf;
    #error_page   404   /404.html;

    # Deny access to PHP files in specific directory
    #location ~ /(wp-content|uploads|wp-includes|images)/.*\.php$ { deny all; }

    include enable-php.conf;

    # 强制跳转到https
    # return 301 https://$http_host$request_uri;

    # 通过nginx代理gogs
    location / {
        proxy_pass http://127.0.0.1:3000/;
        proxy_redirect default;
    }

    location ~ .*\.(gif|jpg|jpeg|png|bmp|swf)$
    {
        expires      30d;
    }

    location ~ .*\.(js|css)?$
    {
        expires      12h;
    }

    location ~ /.well-known {
        allow all;
    }

    location ~ /\.
    {
        deny all;
    }

    access_log off;
}

```
3. 通过git用户执行`./gogs web`启动gogs，这样的只能用来测试，如果想在服务器后台运行则执行这个命令`./gogs web &`
现在可以通过浏览器访问`http://gogstest.xxxxx.top`就看到效果！

### 其他配置
因为我的服务器是1核1G内存，所以我需要设置每天重启服务器并且自动运行gogs
1. 每天定时重启
```
crontab -e

0 3 * * * /sbin/reboot
```
这里通过crontab定时任务来完成重启

2. 开机自启动gogs
```
vim /etc/rc.local

logdate="`date +%Y-%m-%d`"
/bin/su - git -c "/usr/bin/nohup /home/wwwroot/gogs/gogs web > '/home/wwwroot/gogs/log/'${logdate}'.log' 2>&1 &"
```
这里通过开机任务来切换git用户启动gogs，记录每天的日志。