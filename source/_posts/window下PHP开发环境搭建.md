---
title: window下PHP开发环境搭建
date: 2018-10-31 21:59:43
categories: "编程"
tags:
    - php
---
window下推荐使用集成环境，方便快捷
<!--more-->
# window下PHP开发环境搭建

## [php集成环境-phpstudy](http://phpstudy.php.cn/)

> 一次性安装，无须配置即可使用，是非常方便、好用的PHP调试环境。
该程序绿色小巧简易迷你仅有32M，有专门的控制面板。总之学习PHP只需一个包。
对学习PHP的新手来说，WINDOWS下环境配置是一件很困难的事；对老手来说也是一件烦琐的事。
因此无论你是新手还是老手，该程序包都是一个不错的选择。
全面适合 Win2000/XP/2003/win7/win8/win2008 操作系统
支持Apache、IIS、Nginx和LightTPD。

## Composer安装 [`传送门`](https://getcomposer.org/Composer-Setup.exe)

## php常用扩展

### Redis
1. 下载redis的dll文件 [`传送门`](https://windows.php.net/downloads/pecl/releases/redis)
2. `php.ini` 增加 *extension=php_redis.dll*
3. 下载redis的服务端和客户端 [`传送门`](https://github.com/MicrosoftArchive/redis/releases)
4. 开机自启动
- 在redis的目录下执行（执行后就作为windows服务了）
    ```bash
    redis-server --service-install redis.windows.conf
    ```

- 安装好后需要手动启动redis
    ```bash
    redis-server --service-start
    ```

- 停止服务
    ```bash
    redis-server --service-start
    ```

- 卸载redis服务
    ```bash
    redis-server --service-uninstall
    ```

### Memcache
1. 下载Memcache的dll文件 [`传送门`](https://github.com/nono303/PHP7-memcahe-dll/tree/master)，其他版本 [`点击跳转`]('https://windows.php.net/downloads/pecl/releases/memcache/')
2. `php.ini` 增加 *extension=php_memcache.dll*
3. 下载memcache的服务端并安装 [`传送门`](http://www.runoob.com/memcached/window-install-memcached.html)