---
title: python爬取猫眼正在热映电影
date: 2019-04-29 11:20:24
tags:
    - python
---
python网络爬虫
<!-- more -->
> 用`python`写爬虫爬取需要的数据比较容易，以`Python`简洁的语法和一大波成熟的库，写起来相当的快

**python的版本以及使用的库**
- Python 3.6.4
- requests
- lxml
---
这次主要是爬取猫眼的热映电影，网址为`http://maoyan.com/films`
##### 新建个项目
我用 **类(Class)** 的方式创建，并初始化需要使用的参数
```python
class MaoYanHot:

    def __init__(self):
        self.session = requests.session()  # requests库的session对象能够帮我们跨请求保持某些参数，也会在同一个session实例发出的所有请求之间保持cookies
        self.baseUrl = 'http://maoyan.com/films'
        # 设置requests请求的headers参数来模拟浏览器访问
        self.headers = {
            'User-Agent': 'Mozilla/5.0 (Windows NT 6.1) AppleWebKit/537.11 (KHTML, like Gecko) Chrome/23.0.1271.64 Safari/537.11',
            'Accept': 'text/html;q=0.9,*/*;q=0.8',
            'Accept-Charset': 'ISO-8859-1,utf-8;q=0.7,*;q=0.3',
            'Accept-Encoding': 'gzip',
            'Connection': 'close',
            'Referer': None  # 注意如果依然不能抓取的话，这里可以设置抓取网站的host
        }

    # 开始访问
    def run(self, pages='?offset=0'):
        url = self.baseUrl + pages
        print('获取url：' + url)


MaoYanHot = MaoYanHot()
MaoYanHot.run()
```
#### 获取正在热映电影首页数据
写个`def`来获取网页内容，并在`run`里面调用
```python
# 获取网页内容
def getHtmlContent(self, url):
    try:
        respone = self.session.get(url, headers=self.headers)
        if respone.status_code == 200:
            # 这里的编码取决于网页编码
            # 在decode的时候指定错误处理方式,有strict, ignore和replace三种处理方式，strict是默认的也就是抛出异常，ignore是忽略非法字符
            return respone.content.decode('utf-8', 'ignore')
        return False
    except Exception as e:
        print('err1:' + str(e))
        return False
```
在**run**里面调用`contents = self.getHtmlContent(url)`

如果顺利的话就会得到HTML内容，通过HTML发现dd标签并没有闭合需要处理下
![](https://user-gold-cdn.xitu.io/2018/5/17/1636ea7b2ac1d8cf?w=795&h=347&f=png&s=51601)
通过`etree.HTML`我发现并不能正确闭合dd标签，造成层层嵌套，写个例子测试下
```python
from lxml import etree

aaa = '''
<dl>
    <dd>1111
    <dd>1111
    <dd>1111
    <dd>1111
</dl>
'''
bbb = etree.HTML(aaa)
print(etree.tostring(bbb))
```
打印的结果是`b'<html><body><dl>\n    <dd>1111\n    <dd>1111\n    <dd>1111\n    <dd>1111\n</dd></dd></dd></dd></dl>\n</body></html>'`

#### 把HTML转为XML
手动闭合`dd`后通过`etree.HTML`把HTML转为XML，利用[`xpath语法`](http://www.runoob.com/xpath/xpath-tutorial.html)可以快速匹配我们需要的节点。
```python
# 把html转为xml
def htmlToXml(self, contents):
    # 手动闭合dd标签
    contents = contents.replace('<dd', '</dd><dd')
    return etree.HTML(contents)
```
在**run**里面调用`xmls = self.htmlToXml(contents)`
再来看看电影结构组成，如下图
![](https://user-gold-cdn.xitu.io/2018/5/17/1636eb34a66c1149?w=540&h=402&f=png&s=22165)
我们可以看到电影数据都是在`dd`里面，评分分为 *暂无评分* 和 *具体评分* 2种，父元素是`dl`，而它的`class[movie-list]`在页面只有一个。

#### 获取电影的名称和评分
接下来写个`def`分析需要的电影数据，如电影的名称和评分
```python
# 利用xpath解析html内容
    def parsingHtml(self, html):
        try:
            # 选择文档中的节点，而不考虑它们的位置。选取属性class="movie-list"的 dl 节点，这里返回的是dd列表集合
            movieList = html.xpath('//dl[@class="movie-list"]/dd')

            if len(movieList):
                for item in movieList:
                    # 从当前的 dd 节点选取属性data-act="movies-click"的 a 的节点的文本内容，这里匹配的是电影名称
                    movieName = item.xpath('.//a[@data-act="movies-click"]/text()')[0]
                    # 从当前的 dd 节点选取属性class="channel-detail channel-detail-orange的 div 的节点的文本内容，这里匹配的是暂无评分
                    moviePa = item.xpath('.//div[@class="channel-detail channel-detail-orange"]/text()')
                    # moviePa没有值说明是有具体评分
                    if not moviePa:
                        # 匹配评分的整数
                        integer = item.xpath('.//div[@class="channel-detail channel-detail-orange"]/i[@class="integer"]/text()')
                        # 匹配评分的小数
                        fraction = item.xpath('./div[@class="channel-detail channel-detail-orange"]/i[@class="fraction"]/text()')
                        moviePa = integer + fraction
                    yield({
                        'movieName': movieName,
                        'moviePa': ''.join(moviePa)
                    })
            return
        except Exception as e:
            print('err2:' + str(e))
            return
```
在**run**里面调用`movieList = self.parsingHtml(xmls)`，通过`for`循环打印电影数据
```
for item in movieList:
    strs = '电影：%s -- 评分： %s' % (item['movieName'], item['moviePa'])
    print(strs)
```
输出如下:
```
获取url：http://maoyan.com/films?offset=0
电影：复仇者联盟3：无限战争 -- 评分： 8.6
电影：我是你妈 -- 评分： 8.2
电影：后来的我们 -- 评分： 8.1
电影：超时空同居 -- 评分： 8.8
电影：寂静之地 -- 评分： 暂无评分
电影：幕后玩家 -- 评分： 8.1
电影：狂暴巨兽 -- 评分： 9.0
电影：战犬瑞克斯 -- 评分： 8.2
电影：巴霍巴利王2：终结 -- 评分： 8.7
电影：亲爱的，我要和别人结婚了 -- 评分： 8.2
电影：小公主艾薇拉与神秘王国 -- 评分： 5.7
电影：昼颜 -- 评分： 暂无评分
电影：头号玩家 -- 评分： 9.1
电影：七号公馆 -- 评分： 暂无评分
电影：冰雪女王3：火与冰 -- 评分： 8.2
电影：侏罗纪世界2 -- 评分： 暂无评分
电影：玛丽与魔女之花 -- 评分： 8.0
电影：复仇者联盟4 -- 评分： 暂无评分
电影：青年马克思 -- 评分： 8.4
电影：狄仁杰之四大天王 -- 评分： 暂无评分
电影：爱情公寓 -- 评分： 暂无评分
电影：路过未来 -- 评分： 暂无评分
电影：21克拉 -- 评分： 8.2
电影：游侠索罗：星球大战外传 -- 评分： 暂无评分
电影：午夜十二点 -- 评分： 3.5
电影：龙在哪里？ -- 评分： 8.6
电影：火魔高跟鞋 -- 评分： 暂无评分
电影：动物世界 -- 评分： 暂无评分
电影：犬之岛 -- 评分： 7.9
电影：厉害了，我的国 -- 评分： 9.6
```
#### 自动获取下一页的数据
自此已经可以请求网页并且获取需要的数据，接下来自动请求下一页。通过分析发现URl都是`http://maoyan.com/films`拼接`?offset=x`这个参数，x是当前页数的起始数，第一页`x`为0，第二页`x`为30，以此类推。刚好下一页的`href`就是这个参数。

写个`def`获取下一个的参数
```python
# 匹配下一页
def getNextPage(self, html):
    try:
        # 获取a元素的文本值为下一页的href属性
        result = html.xpath('//a[text()="下一页"]/@href')
        return result[0] if len(result) else False
    except Exception as e:
        print('err3:' + str(e))
        return False
```
在**run**里面通过递归的方式实现自动获取下一页的数据，并随机休眠5-20秒以预防被猫眼拒绝访问
```python
NextPage = self.getNextPage(xmls)
    if NextPage:
        sleepTime = random.randint(5, 20)
        print('休眠时间：%s' % (sleepTime))
        # time.sleep(sleepTime)
        sarr = list(range(sleepTime))
        sarr.sort(reverse=True)
        for iss in sarr:
            time.sleep(1)
            print('倒计时：' + str(iss))
        self.run(NextPage)
```
到这里已经基本完成了一个爬虫

#### 设置cookie来获取需要登陆的网站的数据
有些网站内容必须需要登陆了才能访问，手动设置cookie的方式应该是比较省力的，通过浏览器登陆网页后，通过`F12` - `Network` - `Headers` - `Request Headers` 找到 `Cookie` 字段里面包含了cookie信息，写个`def`把字符串转为字典
```python
def dictToCookie(self):
    cookie_str = 'uuid=1A6E888B4A4B29B16FBA1299108DBE9C716675A309C3BAA9D55E347DEB6E83EC; _lxsdk_cuid=16367a2e6a9c8-0d58eea69a4d89-3c3c5905-1fa400-16367a2e6a9c8; _lxsdk=1A6E888B4A4B29B16FBA1299108DBE9C716675A309C3BAA9D55E347DEB6E83EC; _csrf=89ce8482c620a863330c9ffcaf2e20579a55a76cbd8637ac62a2b9f72c172a7b; _lx_utm=utm_source%3DBaidu%26utm_medium%3Dorganic; lt=DiIgzaThdLC5XZdcq4joMkZRpD8AAAAA3QUAAIpNTDuzpXNwUmtKXTXO6i9RayLLEM7KoYPwtHI1-tbRoMyue5EEa87kRKXrkA9SxQ; lt.sig=HiWqeKJoko8oWV-YaVVbIHt4iXQ; __mta=46043675.1526452119497.1526534209978.1526537087635.59; _lxsdk_s=1636cb36a83-aff-08c-b8a%7C%7C2'
    cookie_dict = dict()
    for item in cookie_str.split(';'):
        key, value = item.split('=', 1)  # 1代表只分一次，得到两个数据
        cookie_dict[key] = value
    return cookie_dict
```
然后在`class`的`__init__`里面把字典转为`cookiejar`并设置`cookie`
```python
cookie_dict = self.dictToCookie()
self.session.cookies = requests.utils.cookiejar_from_dict(cookie_dict, cookiejar=None, overwrite=True)
```
#### 完整代码
```python
#!/usr/bin/env python
# -*- coding: utf-8 -*-
# @Date    : 2018-05-16 15:14:06
# @Author  : Your Name (you@example.org)
# @Link    : link
# @Version : 1.0.0
# 获取猫眼热门电影

import time
import requests
import random
from lxml import etree


class MaoYanHot:
    def __init__(self):
        self.session = requests.session()
        self.baseUrl = 'http://maoyan.com/films'
        self.headers = {
            'User-Agent': 'Mozilla/5.0 (Windows NT 6.1) AppleWebKit/537.11 (KHTML, like Gecko) Chrome/23.0.1271.64 Safari/537.11',
            'Accept': 'text/html;q=0.9,*/*;q=0.8',
            'Accept-Charset': 'ISO-8859-1,utf-8;q=0.7,*;q=0.3',
            'Accept-Encoding': 'gzip',
            'Connection': 'close',
            'Referer': None  # 注意如果依然不能抓取的话，这里可以设置抓取网站的host
        }
        cookie_dict = self.dictToCookie()
        self.session.cookies = requests.utils.cookiejar_from_dict(cookie_dict, cookiejar=None, overwrite=True)

    # 转为字典
    def dictToCookie(self):
        cookie_str = 'uuid=1A6E888B4A4B29B16FBA1299108DBE9C716675A309C3BAA9D55E347DEB6E83EC; _lxsdk_cuid=16367a2e6a9c8-0d58eea69a4d89-3c3c5905-1fa400-16367a2e6a9c8; _lxsdk=1A6E888B4A4B29B16FBA1299108DBE9C716675A309C3BAA9D55E347DEB6E83EC; _csrf=89ce8482c620a863330c9ffcaf2e20579a55a76cbd8637ac62a2b9f72c172a7b; _lx_utm=utm_source%3DBaidu%26utm_medium%3Dorganic; lt=DiIgzaThdLC5XZdcq4joMkZRpD8AAAAA3QUAAIpNTDuzpXNwUmtKXTXO6i9RayLLEM7KoYPwtHI1-tbRoMyue5EEa87kRKXrkA9SxQ; lt.sig=HiWqeKJoko8oWV-YaVVbIHt4iXQ; __mta=46043675.1526452119497.1526534209978.1526537087635.59; _lxsdk_s=1636cb36a83-aff-08c-b8a%7C%7C2'
        cookie_dict = dict()
        for item in cookie_str.split(';'):
            key, value = item.split('=', 1)  # 1代表只分一次，得到两个数据
            cookie_dict[key] = value
        return cookie_dict

    # 获取网页内容
    def getHtmlContent(self, url):
        try:
            respone = self.session.get(url, headers=self.headers)
            if respone.status_code == 200:
                return respone.content.decode('utf-8')
            return False
        except Exception as e:
            print('err1:' + str(e))
            return False

    # 把html转为xml
    def htmlToXml(self, contents):
        # 手动闭合dd标签
        contents = contents.replace('<dd', '</dd><dd')
        return etree.HTML(contents)

    # 利用xpath解析html内容
    def parsingHtml(self, html):
        try:
            movieList = html.xpath('//dl[@class="movie-list"]/dd')

            if len(movieList):
                for item in movieList:
                    movieName = item.xpath('.//a[@data-act="movies-click"]/text()')[0]
                    moviePa = item.xpath('.//div[@class="channel-detail channel-detail-orange"]/text()')
                    if not moviePa:
                        integer = item.xpath('.//div[@class="channel-detail channel-detail-orange"]/i[@class="integer"]/text()')
                        fraction = item.xpath('./div[@class="channel-detail channel-detail-orange"]/i[@class="fraction"]/text()')
                        moviePa = integer + fraction
                    yield({
                        'movieName': movieName,
                        'moviePa': ''.join(moviePa)
                    })
            return
        except Exception as e:
            print('err2:' + str(e))
            return

    # 匹配下一页
    def getNextPage(self, html):
        try:
            result = html.xpath('//a[text()="下一页"]/@href')
            return result[0] if len(result) else False
        except Exception as e:
            print('err3:' + str(e))
            return False

    # 开始访问
    def run(self, pages='?offset=18180'):
        url = self.baseUrl + pages
        print('获取url：' + url)
        contents = self.getHtmlContent(url)
        if contents:
            xmls = self.htmlToXml(contents)
            movieList = self.parsingHtml(xmls)

            file_object = open('movie.txt', 'a+', encoding='utf-8')
            file_object.write('获取url：' + url + "\n")
            for item in movieList:
                strs = '电影：%s -- 评分： %s' % (item['movieName'], item['moviePa'])
                print(strs)
                file_object.write(strs + "\n")
            file_object.close()

            NextPage = self.getNextPage(xmls)
            if NextPage:
                sleepTime = random.randint(5, 20)
                print('休眠时间：%s' % (sleepTime))
                # time.sleep(sleepTime)
                sarr = list(range(sleepTime))
                sarr.sort(reverse=True)
                for iss in sarr:
                    time.sleep(1)
                    print('倒计时：' + str(iss))
                self.run(NextPage)
        else:
            print('网页内容获取失败')


MaoYanHot = MaoYanHot()
MaoYanHot.run()

```