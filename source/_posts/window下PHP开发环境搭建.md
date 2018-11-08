---
title: window下PHP开发环境搭建
date: 2018-10-31 21:59:43
categories: "编程"
tags:
    - php
---
# window下PHP开发环境搭建

------

## [php集成环境-phpstudy](http://phpstudy.php.cn/)

<!--more-->
> 一次性安装，无须配置即可使用，是非常方便、好用的PHP调试环境。
该程序绿色小巧简易迷你仅有32M，有专门的控制面板。总之学习PHP只需一个包。
对学习PHP的新手来说，WINDOWS下环境配置是一件很困难的事；对老手来说也是一件烦琐的事。
因此无论你是新手还是老手，该程序包都是一个不错的选择。
全面适合 Win2000/XP/2003/win7/win8/win2008 操作系统
支持Apache、IIS、Nginx和LightTPD。

------
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
1. 下载Memcache的dll文件 [`传送门`](https://github.com/nono303/PHP7-memcahe-dll/tree/master)
2. `php.ini` 增加 *extension=php_memcache.dll*
3. 下载memcache的服务端并安装 [`传送门`](http://www.runoob.com/memcached/window-install-memcached.html)

## Sublime Text 3
### 个人配置
```json
{
    // 字体大小
    "font_size": 14,
    // 高亮编辑中的那一行
    "highlight_line": true,
    // 焦点丢失后自动保存
    "save_on_focus_lost": true,
    // 显示当前文件的编码和行结尾
    "show_encoding": true,
    "show_line_endings": true,
    // 保存的时候把无用的空格去掉
    "trim_trailing_white_space_on_save": true,
    // Tab转换 这个设置会在你按Tab的时候，转成4个空格
    "tab_size": 4,
    "translate_tabs_to_spaces": true,
    // 自动换行
    "word_wrap": false,
    // 宽度指导线
    "rulers": [80],
    // 拼写检查
    "spell_check": false,
    // 要不要滚过头
    "scroll_past_end": true,
    // 显示Tab、空格
    "draw_white_space": "all",
    // 加粗文件夹名称
    "bold_folder_labels": true,
    // 显示全路径
    "show_full_path": true,
    // 设置文件修改时, 标签高亮提示, 这样可以提示保存
    "highlight_modified_tabs": true,
}
```

### 主题
- [Spacegray](https://github.com/kkga/spacegray)
```json
{
    /*主题设置*/
    "theme": "Spacegray Eighties.sublime-theme",
    "color_scheme": "Packages/Theme - Spacegray/base16-eighties.dark.tmTheme",
    // 标签字体大小
    "spacegray_tabs_font_large": true,
    // 标签高度
    "spacegray_tabs_large": true,
    // 边栏标签字体大小
    "spacegray_sidebar_font_large": true,
    // 启用侧栏文件图标
    "spacegray_fileicons": true,
}
```
- [Predawn](https://github.com/jamiewilson/predawn)
- [Material Theme](https://github.com/equinusocio/material-theme)

### 插件
- Alignment
代码对齐，如写几个变量，选中这几行，Ctrl+Alt+A，哇，齐了。
- SideBarEnhancements
侧栏右键功能增强，非常实用
- BracketHighlighter
该插件提供行数列高亮的各种配对的语法符号。
- DocBlockr
一个真正简单的方式来轻松地创建许多语言包括JavaScript，PHP和CoffeeScript的文档块。只要在函数的上面输入`/**`，按Tab就可以了。DocBlockr会观察函数需要的变量名和类型，并创建文档块。
- SublimeTmpl
快速生成文件模板
- emmet
前端神器。一个可以极大提高web开发者HTML和CSS工作效率的工具箱组件。
- jQuery
为jQuery的大部分方法提供了示例代码段，让jQuery的API更加容易使用。