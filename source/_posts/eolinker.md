---
title: eolinker
date: 2019-04-02 21:37:18
categories: "杂项"
tags:
---
eoLinker 是目前业内领先、国内最大的在线 API 接口管理平台，提供自动生成 API 文档、API 自动化测试、Mock 测试、团队协作等功能，旨在解决由于前后端分离导致的开发效率低下问题。
<!-- more -->
### 快速开始
在`Cmder`输入下面的命令克隆项目
```bash
git clone https://github.com/eolinker/PHP-EOLINKER-API-Studio-Lite-5.0.git
```
### 环境要求
- PHP 5.5+ / PHP7+（推荐）
- Nginx（推荐） / Apache
- 项目目录需要具有完全的读写权限（777），用于写入配置文件。安装完成之后可以设置另外设置目录权限
- PHP需要安装并启用mbstring以及curl模块，用于字符串处理以及接口测试功能
### 目录结构
```bash
PHP-EOLINKER-API-Studio-Lite-5.0
├─ backend_source_code     |php源码
├─ frontend_source_code    |前端源码
├─ release                 |正式安装包
├─ LICENSE
└─ README.md
```
### 安装依赖
```bash
cd frontend_source_code
#安装前端依赖
npm install
#安装bower，如已安装忽略
npm install bower -g
#安装运行依赖
bower install
```
#### 无法安装node-sass
如果遇到以下情况无法安装node-sass的需要手动下载 `win32-x64-64_binding.node`
```bash
npm ERR! code ELIFECYCLE
npm ERR! errno 1
npm ERR! node-sass@3.13.1 postinstall: `node scripts/build.js`
npm ERR! Exit status 1
npm ERR!
npm ERR! Failed at the node-sass@3.13.1 postinstall script.
npm ERR! This is probably not a problem with npm. There is likely additional logging output above.

npm ERR! A complete log of this run can be found in:
npm ERR!     C:\Users\Draco\AppData\Roaming\npm-cache\_logs\2019-03-28T14_53_20_124Z-debug.log
```
使用以下命令查看对应的版本
```bash
node -p "[process.platform, process.arch, process.versions.modules].join('-')"
```
然后到[`https://github.com/sass/node-sass/releases`](https://github.com/sass/node-sass/releases)下载对应的版本，并且在电脑新建环境变量，键为`SASS_BINARY_PATH`，值为文件存放的地址，如 E:\win32-x64-64_binding.node，然后重新打开`Cmder`执行`npm install`命令

### 调试
先执行`npm install gulp@3.9.* -g`安装gulp，因为项目里的写法不支持gulp4，所以安装3.9版本
```bash
#开发模式
gulp serve
#编译模式（将项目文件输出为上线文件）
gulp build
#调试上线模式
gulp serve:dist
```
#### 安装Ruby and Compass
```bash
[23:41:58] Starting 'styles:compass'...
[23:41:59] Error in plugin 'gulp-compass'
Message:
    You need to have Ruby and Compass installed and in your system PATH for this task to work.
[23:41:59] Tested 12 tests, 12 passes, 0 failures: PASS
```
如果出现这个提示，则需要安装Ruby和Compass，并且把Ruby添加到环境变量，如键为`Ruby`，值为`G:\Ruby26-x64\bin`，
执行`gem install compass`命令安装compass
#### 设置sass的缓存目录
```bash
[23:54:24] all files 3.05 MB
[23:54:24] Finished 'jshint' after 3.54 s
    error src/app/import.scss (G:/Ruby26-x64/lib/ruby/2.6.0/tempfile.rb:133:in `initialize': No such file or director
y @ rb_sysopen - F:/PHP-EOLINKER-API-Studio-Lite-5.0/frontend_source_code/.sass-cache/75fcaf1b4852ceb732871195e41567c
c2a7d8997/F%058/PHP-EOLINKER-API-Studio-Lite-5.0/frontend_source_code/src/app/ui/content/home/project/inside/content/
env/default/index.scssc20190328-7424-2aelrr)
Compilation failed in 1 files.
[23:54:25] Error in plugin 'gulp-compass'
Message:
    directory .tmp/serve/app
    error src/app/import.scss (G:/Ruby26-x64/lib/ruby/2.6.0/tempfile.rb:133:in `initialize': No such file or director
y @ rb_sysopen - F:/PHP-EOLINKER-API-Studio-Lite-5.0/frontend_source_code/.sass-cache/75fcaf1b4852ceb732871195e41567c
c2a7d8997/F%058/PHP-EOLINKER-API-Studio-Lite-5.0/frontend_source_code/src/app/ui/content/home/project/inside/content/
env/default/index.scssc20190328-7424-2aelrr)
Compilation failed in 1 files.
```
这个问题是由sass-cache目录路径过长，compass操作不过来引起的，在config.rb中添加cache_path = 'F:\temp\sass'即可，也可以是其他路径，只要路径长度不超过255就行
### eoLinker前端修改
#### 语言换成中文
1. 找到文件`frontend_source_code/src/app/app.module.js`以下配置
```javascript
$logProvider.debugEnabled(IsDebug);
$urlRouterProvider.otherwise('/index');
$translateProvider.translations('en', EN);
$translateProvider.preferredLanguage('en');
```
改成如下
```javascript
$logProvider.debugEnabled(IsDebug);
$urlRouterProvider.otherwise('/index');
// $translateProvider.translations('en', EN);
// $translateProvider.preferredLanguage('en');
$translateProvider.translations('cn', CN);
$translateProvider.preferredLanguage('cn');
```
2. 展开中文国际化文件，文件为 `frontend_source_code/src/app/constant/language/cn.constants.js`，并且删除中文国际化文件中无意义的“0”前缀
#### 其他修复
1. 修复登录页“用户名”显示不正确，placeholder错误导致登录页“用户名”显示为“undefined”
```
#文件：`frontend_source_code/src/app/ui/content/index/index.html`
#原内容
`<input class="eo-input" name="username" data-ng-model="$ctrl.data.interaction.request.loginName" type="text" placeholder="{{'undefined' }}" ng-class="{'eo-input-error':loginForm.username.$invalid&&$ctrl.data.info.submitted}" required autofocus />`
#修改后
`<input class="eo-input" name="username" data-ng-model="$ctrl.data.interaction.request.loginName" type="text" placeholder="{{'21'|translate}}" ng-class="{'eo-input-error':loginForm.username.$invalid&&$ctrl.data.info.submitted}" required autofocus />`
```
2. 修复接口编辑页点击“更多设置”按钮报错，三元表达式和国际化组合使用时的代码有误导致弹窗无法渲染
```
#文件：`frontend_source_code/src/app/modal/branch/ams/index.html`
#原内容
`<header class="eo-modal-header">{{data.input.status=='body'?'37'|translate:data.input.status=='restful'?'38'|translate:'39'|translate}}{{'40'|translate}}</header>`
#修改后
`<header class="eo-modal-header">{{(data.input.status=='body'?'37':data.input.status=='restful'?'38':'39')|translate}}{{'40'|translate}}</header>`

#原内容
`<td>{{data.input.item.paramNotNull?'45'|translate:''}}</td>`
#修改后
`<td>{{data.input.item.paramNotNull?('45'|translate):''}}</td>`
```
3. 修复未在cacheJson中存储apiNote及相关字段的信息，apiNote及相关字段的信息被保存在了api表中，但未保存到apiCache表中
```
#文件：`backend_source_code/Server/Web/Module/ApiModule.class.php`
#112行
$cacheJson['baseInfo']['apiSuccessStatusCode'] = $success_status_code;
# 增加以下内容
$cacheJson['baseInfo']['apiNoteType'] = $apiNoteType;
$cacheJson['baseInfo']['apiNote'] = $apiNote;
$cacheJson['baseInfo']['apiNoteRaw'] = $apiNoteRaw;
#228行
$cacheJson['baseInfo']['apiSuccessStatusCode'] = $success_status_code;
#增加以下内容
```
4. 注释掉对未定义的方法的调用
```
#文件：`frontend_source_code/src/app/ui/content/home/project/inside/content/api/edit/index.js`
#517行
if (vm.data.reset.status != 'add') {
    fun.monitorEditParam(status, arg.item);
}

#655
if (vm.data.reset.status != 'add') {
    fun.monitorDeleteParam(status, arg.item, arg.$index);
}
```
5. 修复无法保存接口，后端未返回必须的字段，没有做兼容导致无法保存（本修复是不必要的，只要后端返回了这三个字段就好了）
```
#文件：`frontend_source_code/src/app/ui/content/home/project/inside/content/api/edit/index.js`
#1276
vm.interaction.response.apiInfo.apiNoteType = "" + vm.interaction.response.apiInfo.apiNoteType;
vm.interaction.response.apiInfo.apiRichNote = vm.interaction.response.apiInfo.apiNoteType == '0' ? vm.interaction.response.apiInfo.apiNote : '';
vm.interaction.response.apiInfo.apiMarkdownNote = vm.interaction.response.apiInfo.apiNoteType == '1' ? vm.interaction.response.apiInfo.apiNote : '';
#修改后
vm.interaction.response.apiInfo.apiNoteType = "" + fun.getOrDefault(vm.interaction.response.apiInfo.apiNoteType, '1');
vm.interaction.response.apiInfo.apiNoteRaw = fun.getOrDefault(vm.interaction.response.apiInfo.apiNoteRaw, '');
vm.interaction.response.apiInfo.apiRichNote = vm.interaction.response.apiInfo.apiNoteType == '0' ? fun.getOrDefault(vm.interaction.response.apiInfo.apiNote, '') : '';
vm.interaction.response.apiInfo.apiMarkdownNote = vm.interaction.response.apiInfo.apiNoteType == '1' ? fun.getOrDefault(vm.interaction.response.apiInfo.apiNote, '') : '';

#1379行后增加
fun.getOrDefault = function (optional, defaultValue) {
    if (optional == undefined) {
        return defaultValue;
    }
    return optional;
}
```

### 整合前后端
1. 在php的web目录新建文件夹`eolinker_os`作为项目根目录，新建个文件夹存放`backend_source_code`的所有后端文件，
在`eolinker_os\server\RTP`新建目录`config`，在`eolinker_os\server`新建目录`dump`
修改`frontend_source_code/app.conf.json`文件的 *development* 和 *production*的字段 serverUrl 为 `/server/index.php`
2. 运行`gulp build`编译前端文件，会在`frontend_source_code`下生成文件夹`eolinker`，把`eolinker`里的所有文件复制到`eolinker_os`，把`eolinker_os`整个文件夹放到服务器部署即可！