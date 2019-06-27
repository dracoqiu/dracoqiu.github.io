---
title: python调用微信扫码登录获取朋友列表
date: 2019-06-27 16:25:30
tags:
    - python
---
通过分析微信网页版登录相关接口实现用python扫描登录获取朋友列表（window下测试）。
<!-- more -->
### 主线程
```python
#!/usr/bin/env python
# -*- coding: utf-8 -*-
# @Date    : 2018-04-12 11:05:31
# @Author  : Your Name (you@example.org)
# @Link    : link
# @Version : 1.0.0

import os
import sys
import requests
import time
import re
import subprocess
import xml.dom.minidom
import json


class WebwxLogin(object):
    # 初始化参数
    def __init__(self):
        self.session = requests.session()
        self.headers = {
            'User-Agent': 'Mozilla/5.0 (Windows NT 5.1; rv:33.0) Gecko/20100101 Firefox/33.0'}
        self.QRImgPath = os.path.split(os.path.realpath(__file__))[0] + os.sep + 'webWeixinQr.jpg'
        self.uuid = ''
        self.tip = 0
        self.base_uri = 'https://wx2.qq.com'
        self.redirect_uri = ''
        self.skey = ''
        self.wxsid = ''
        self.wxuin = ''
        self.pass_ticket = ''
        self.deviceId = 'e000000000000000'
        self.BaseRequest = {}
        self.ContactList = []
        self.My = []
        self.SyncKey = ''

    # 获取UUID
    def getUUID(self):
        url = 'https://login.weixin.qq.com/jslogin'
        params = {
            'appid': 'wx782c26e4c19acffb',
            'redirect_uri': 'https://wx2.qq.com/cgi-bin/mmwebwx-bin/webwxnewloginpage',
            'fun': 'new',
            'lang': 'zh_CN',
            '_': int(time.time() * 1000)
        }
        response = self.session.get(url, params=params, verify=False)
        target = response.content.decode('utf-8')
        print(target)
        pattern = 'window.QRLogin.code = (\d+); window.QRLogin.uuid = "(.*)"'
        ob = re.search(pattern, target)
        code = ob.group(1)
        self.uuid = ob.group(2)
        if code == '200':
            return True
        return False

    # 获取二维码
    def showQRImage(self):
        url = 'https://login.weixin.qq.com/qrcode/' + self.uuid
        response = self.session.get(url, verify=False)
        self.tip = 1
        with open(self.QRImgPath, 'wb') as f:
            f.write(response.content)
            f.close()

        # 打开二维码
        if sys.platform.find('darwin') >= 0:
            subprocess.call(['open', self.QRImgPath])  # 苹果系统
        elif sys.platform.find('linux') >= 0:
            subprocess.call(['xdg-open', self.QRImgPath])  # linux系统
        else:
            os.startfile(self.QRImgPath)  # windows系统
        print('请使用微信扫描二维码登录')

    # 检测登录状态
    def checkLogin(self):
        url = 'https://login.wx2.qq.com/cgi-bin/mmwebwx-bin/login'
        params = {
            'loginicon': 'true',
            'uuid': self.uuid,
            'tip': self.tip,
            '_': int(time.time() * 1000)
        }
        response = self.session.get(url, params=params, verify=False)
        target = response.content.decode('utf-8')
        pattern = 'window.code=(\d+);'
        ob = re.search(pattern, target)
        code = ob.group(1)
        if code == '201':  # 已扫描
            print('成功扫描,请在手机上点击确认登录')
            self.tip = 0
        elif code == '200':  # 已登录
            print('正在登录中...')
            regx = 'window.redirect_uri="(\S+?)";'
            ob = re.search(regx, target)
            self.redirect_uri = ob.group(1) + '&fun=new'
            self.base_uri = self.redirect_uri[:self.redirect_uri.rfind('/')]
            print(self.base_uri)
        elif code == '408':  # 超时
            pass
        return code

    # 登录
    def login(self):
        response = self.session.get(self.redirect_uri, verify=False)
        data = response.content.decode('utf-8')
        # data = '<error><ret>0</ret><message></message><skey>@crypt_609947c9_e2770547725ade68fec8429b023b124f</skey><wxsid>OxjDKFg0ElOfXgsO</wxsid><wxuin>1744069411</wxuin><pass_ticket>4uKemtRtpCiuOdIb%2B0n9KLuxKcjajfWIYt4JWJFxOaNvJACjm%2B%2BvO8ET0OqPzqkx</pass_ticket><isgrayscale>1</isgrayscale></error>'
        print(data)
        doc = xml.dom.minidom.parseString(data)
        root = doc.documentElement
        # 提取响应中的参数
        for node in root.childNodes:
            if node.nodeName == 'skey':
                self.skey = node.childNodes[0].data
            elif node.nodeName == 'wxsid':
                self.wxsid = node.childNodes[0].data
            elif node.nodeName == 'wxuin':
                self.wxuin = node.childNodes[0].data
            elif node.nodeName == 'pass_ticket':
                self.pass_ticket = node.childNodes[0].data

        if not all((self.skey, self.wxsid, self.wxuin, self.pass_ticket)):
            return False
        self.BaseRequest = {
            'Uin': int(self.wxuin),
            'Sid': self.wxsid,
            'Skey': self.skey,
            'DeviceID': self.deviceId,
        }
        return True

    # 初始化界面
    def webwxinit(self):
        url = self.base_uri + '/webwxinit?r=%s&lang=zh_CN&pass_ticket=%s' % (self.wxuin, self.pass_ticket)
        params = {
            'BaseRequest': self.BaseRequest
        }
        h = self.headers
        h['ContentType'] = 'application/json; charset=UTF-8'
        response = self.session.post(url, json=params, headers=h, verify=False)
        data = response.content.decode('utf-8')
        dic = json.loads(data)
        self.ContactList = dic['ContactList']
        self.My = dic['User']
        SyncKeyList = []
        for item in dic['SyncKey']['List']:
            SyncKeyList.append('%s_%s' % (item['Key'], item['Val']))
        self.SyncKey = '|'.join(SyncKeyList)
        ErrMsg = dic['BaseResponse']['ErrMsg']
        Ret = dic['BaseResponse']['Ret']
        if Ret != 0:
            print(ErrMsg)
            return False
        return True

    # 获取联系人
    def webwxgetcontact(self):
        url = self.base_uri + '/webwxgetcontact'
        print(23432, url)
        params = {
            'lang': 'zh_CN',
            'pass_ticket': self.pass_ticket,
            'r': int(time.time() * 1000),
            'seq': 0,
            'skey': self.skey
        }
        h = self.headers
        h['ContentType'] = 'application/json; charset=UTF-8'
        response = self.session.get(url, params=params, verify=False)
        data = response.content.decode('utf-8')
        dic = json.loads(data)
        MemberList = dic['MemberList']

        # 倒序遍历,不然删除的时候出问题..
        SpecialUsers = ["newsapp", "fmessage", "filehelper", "weibo", "qqmail", "tmessage", "qmessage", "qqsync",
                        "floatbottle", "lbsapp", "shakeapp", "medianote", "qqfriend", "readerapp", "blogapp",
                        "facebookapp", "masssendapp",
                        "meishiapp", "feedsapp", "voip", "blogappweixin", "weixin", "brandsessionholder",
                        "weixinreminder", "wxid_novlwrv3lqwv11", "gh_22b87fa7cb3c", "officialaccounts",
                        "notification_messages", "wxitil", "userexperience_alarm"]
        for i in range(len(MemberList) - 1, -1, -1):
            Member = MemberList[i]
            if Member['VerifyFlag'] & 8 != 0:  # 公众号/服务号
                MemberList.remove(Member)
            elif Member['UserName'] in SpecialUsers:  # 特殊账号
                MemberList.remove(Member)
            elif Member['UserName'].find('@@') != -1:  # 群聊
                MemberList.remove(Member)
            elif Member['UserName'] == self.My['UserName']:  # 自己
                MemberList.remove(Member)

        return MemberList

    # 主程序
    def main(self):
        if not self.getUUID():
            print('获取uuid失败')
            return
        self.showQRImage()

        time.sleep(1)

        while self.checkLogin() != '200':
            pass

        os.remove(self.QRImgPath)

        if not self.login():
            print('登录失败')
            return

        # 登录完成, 下面查询好友
        if not self.webwxinit():
            print('初始化失败')
            return

        MemberList = self.webwxgetcontact()

        print('微信通讯录共%s位好友' % len(MemberList))
        nantop = 0
        nvtop = 0
        wztop = 0

        for x in MemberList:
            # sex = '未知' if x['Sex'] == 0 else '男' if x['Sex'] == 1 else '女'
            # print('昵称:%s, 性别:%s, 备注:%s, 签名:%s' % (x['NickName'], sex, x['RemarkName'], x['Signature']))
            if x['Sex'] == 0:
                wztop = wztop + 1
            elif x['Sex'] == 1:
                nantop = nantop + 1
            else:
                nvtop = nvtop + 1

        print('其中男：%d，女：%d，未知：%d' % (nantop, nvtop, wztop))


if __name__ == '__main__':
    print('开始')
    wx = WebwxLogin()
    wx.main()
```
### 运行结果
```
开始
E:\Python\Python37-32\lib\site-packages\urllib3\connectionpool.py:847: InsecureRequestWarning: Unverified HTTPS request is being made. Adding certificate verification is strongly advised. See: https://urllib3.readthedocs.io/en/latest/advanced-usage.html#ssl-warnings
  InsecureRequestWarning)
window.QRLogin.code = 200; window.QRLogin.uuid = "QYQyezcMFQ==";
E:\Python\Python37-32\lib\site-packages\urllib3\connectionpool.py:847: InsecureRequestWarning: Unverified HTTPS request is being made. Adding certificate verification is strongly advised. See: https://urllib3.readthedocs.io/en/latest/advanced-usage.html#ssl-warnings
  InsecureRequestWarning)
请使用微信扫描二维码登录
E:\Python\Python37-32\lib\site-packages\urllib3\connectionpool.py:847: InsecureRequestWarning: Unverified HTTPS request is being made. Adding certificate verification is strongly advised. See: https://urllib3.readthedocs.io/en/latest/advanced-usage.html#ssl-warnings
  InsecureRequestWarning)
成功扫描,请在手机上点击确认登录
E:\Python\Python37-32\lib\site-packages\urllib3\connectionpool.py:847: InsecureRequestWarning: Unverified HTTPS request is being made. Adding certificate verification is strongly advised. See: https://urllib3.readthedocs.io/en/latest/advanced-usage.html#ssl-warnings
  InsecureRequestWarning)
正在登录中...
https://wx2.qq.com/cgi-bin/mmwebwx-bin
E:\Python\Python37-32\lib\site-packages\urllib3\connectionpool.py:847: InsecureRequestWarning: Unverified HTTPS request is being made. Adding certificate verification is strongly advised. See: https://urllib3.readthedocs.io/en/latest/advanced-usage.html#ssl-warnings
  InsecureRequestWarning)
<error><ret>0</ret><message></message><skey>@crypt_c25c57d7_acd069ae3977d7820c81fe8b3d1c021b</skey><wxsid>TmE0nw5bo2fLJJwt</wxsid><wxuin>2508725506</wxuin><pass_ticket>nIdLgkUjB%2B7CnLC%2Bpn9mhzGpRobtp4rwVKXyAuF46yELAFuGUfVOz0IUVAC8goWr</pass_ticket><isgrayscale>1</isgrayscale></error>
E:\Python\Python37-32\lib\site-packages\urllib3\connectionpool.py:847: InsecureRequestWarning: Unverified HTTPS request is being made. Adding certificate verification is strongly advised. See: https://urllib3.readthedocs.io/en/latest/advanced-usage.html#ssl-warnings
  InsecureRequestWarning)
23432 https://wx2.qq.com/cgi-bin/mmwebwx-bin/webwxgetcontact
E:\Python\Python37-32\lib\site-packages\urllib3\connectionpool.py:847: InsecureRequestWarning: Unverified HTTPS request is being made. Adding certificate verification is strongly advised. See: https://urllib3.readthedocs.io/en/latest/advanced-usage.html#ssl-warnings
  InsecureRequestWarning)
微信通讯录共287位好友
其中男：169，女：100，未知：18
```