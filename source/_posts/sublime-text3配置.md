---
title: sublime text3配置
date: 2019-04-26 22:30:36
tags:
    - 编辑器
---
Sublime Text：一款具有代码高亮、语法提示、自动完成且反应快速的编辑器软件，不仅具有华丽的界面，还支持插件扩展机制，用她来写代码，绝对是一种享受。
<!-- more -->
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
代码对齐，如写几个变量，选中这几行，Ctrl+Alt+A，哇，齐了。推荐使用
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
- HTML-CSS-JS Prettify
代码格式化插件
- HTML5
HTML5标签拓展
- JsFormat
javascript格式化
- CSS Format
CSS格式化
- Tag
HTML格式化
- Brackethighlighter
标签对标记
- SideBarEnhancements
增强型侧边栏
- BufferScroll
代码折叠状态保留
- StyleToken
标记颜色代码功能：
- Emmet
前端神器
- TortoiseSVN
SVN你懂的
- QuoteHTML
把HTML拼接成js插入字符串，神器
- Clipboard Manager
增强型剪贴板，可访问历史剪贴板记录
- FileHeader
文件模板 , 可自动更新修改时间
- AutoPrefixer
浏览器私有属性前缀补全 (Node.js依赖)
- ColorConvert
RGBA颜色转换，十六进制颜色转换为RGBA颜色
- Better Completion
全能代码提示
- LiveStyle
双向更改无刷新实时预览 , 包含chrome插件 Emmet LiveStyle
- jQuery
jQuery 代码提示（Better Completion 已可替代此插件）
- Sass以及SASS Build
使用Sass必备，ctrl+b执行编译