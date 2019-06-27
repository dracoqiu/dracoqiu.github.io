---
title: python获取微信公众号的access_token和ticket
date: 2019-06-27 15:38:38
tags:
    - python
---
微信公众号的access_token和ticket都有次数限制，所以使用python来定时获取数据并保存到redis中，方便多方取值。
<!-- more -->
### 配置设置
```python
#!/usr/bin/env python
# -*- coding: utf-8 -*-
# @Date    : 2017-11-09 16:16:21
# @Author  : Your Name (you@example.org)
# @Link    : http://example.org
# @Version : $Id$

# 定义个人测试公众号的配置
myWeChat = {
    'appID': 'wx89*********ba6',
    'appsecret': 'c69b71**********9ec1e'
}

weChat_api = {
    'access_token': 'https://api.weixin.qq.com/cgi-bin/token?grant_type=client_credential&appid=%s&secret=%s',  # 获取微信token
    'ticket': 'https://api.weixin.qq.com/cgi-bin/ticket/getticket?access_token=%s&type=jsapi',  # 获取微信ticket
}

# 定义redis配置
myRedis = {'host': '127.0.0.1', 'port': 6379, 'db': 5}

redisKey = {
    'access_token': 'wechat_access_token',  # 微信token
    'token_tmp': 'wechat_token_tmp',  # 微信token临时储存
    'ticket': 'wechat_ticket',  # 微信token
    'ticket_tmp': 'wechat_ticket_tmp',  # 微信ticket临时储存
}
```
### 日志记录
```python
#!/usr/bin/env python
# -*- coding: utf-8 -*-
# @Date    : 2017-11-09 16:16:21
# @Author  : Your Name (you@example.org)
# @Link    : http://example.org
# @Version : $Id$

import logging
from logging.handlers import TimedRotatingFileHandler
import time

logfilename = '/home/python/wechat_py/logs/' + str(time.strftime('%Y%m%d')) + '.log'
logging.basicConfig(
    level=logging.INFO,
    format='%(asctime)s %(levelname)s %(message)s',
    datefmt='%d %m %Y %H:%M:%S',
    filename=logfilename,
    filemode='a')

#################################################################################################
# # 定义一个TimedRotatingFileHandler，按时间段写日志，不删除历史日志.服务器定时重启的话请注释这块
# Rthandler = TimedRotatingFileHandler('/home/py/wechat_py/logs/myapp.log', when="D", interval=1, backupCount=0)
# # 设置后缀名称，跟strftime的格式一样
# Rthandler.suffix = "%Y-%m-%d.log"
# Rthandler.setLevel(logging.DEBUG)
# formatter = logging.Formatter('%(asctime)s - %(name)s - %(levelname)s - %(message)s')
# Rthandler.setFormatter(formatter)
# logging.getLogger('').addHandler(Rthandler)
###########################################################################################

#################################################################################################
# 定义一个StreamHandler，将INFO级别或更高的日志信息打印到标准错误，并将其添加到当前的日志处理对象#
console = logging.StreamHandler()
console.setLevel(logging.INFO)
formatter = logging.Formatter('%(asctime)s - %(name)s - %(levelname)s - %(message)s')
console.setFormatter(formatter)
logging.getLogger('').addHandler(console)
#################################################################################################

# logging.debug('This is debug message by liu-ke')
# logging.info('This is info message by liu-ke')
# logging.warning('This is warning message by liu-ke')
```
### 主线程
```python
#!/usr/bin/env python
# -*- coding: utf-8 -*-
# @Date    : 2017-11-09 16:16:21
# @Author  : Your Name (you@example.org)
# @Link    : http://example.org
# @Version : $Id$
from AppLoggin import logging
from AppConfig import myRedis, redisKey, weChat_api, myWeChat
import redis
import datetime
import time
import requests
import json

# 线程休眠时间
smtime = 600
# redis保存时间
settime = 1800

pool = redis.ConnectionPool(**myRedis)
r = redis.Redis(connection_pool=pool)
print('获取公众号token ticket 脚本启动...')

while True:
    try:
        # 获取token
        check_key_token = r.get(redisKey['token_tmp'])
        # print(check_key_token)
        if not check_key_token:
            token_url = weChat_api['access_token'] % (myWeChat['appID'], myWeChat['appsecret'])
            # logging.info(token_url)

            try:
                token_result = requests.get(token_url, verify=False)
                logging.info('token result: %s' % token_result.text)
                token_res = token_result.json()

                if token_res.get('access_token'):
                    access_token = token_res['access_token'].encode('utf-8')

                    r.set(redisKey['access_token'], access_token)
                    r.set(redisKey['token_tmp'], access_token, settime)
                    check_key_token = access_token

                    logging.info('[INFO] set token %s ' % token_res['access_token'])

            except Exception as e:
                print(datetime.datetime.now().strftime("%Y-%m-%d %H:%M:%S"), '[ERROR] get token. ', str(e))

        # 获取ticket
        check_key_ticket = r.get(redisKey['ticket_tmp'])
        if not check_key_ticket and check_key_token:

            ticket_url = weChat_api['ticket'] % check_key_token.decode('utf-8')
            # logging.info(ticket_url)

            try:
                ticket_result = requests.get(ticket_url, verify=False)
                logging.info('ticket result: %s' % ticket_result.text)
                ticket_res = ticket_result.json()
                if ticket_res.get('ticket'):
                    ticket = ticket_res['ticket'].encode('utf-8')

                    r.set(redisKey['ticket'], ticket)
                    r.set(redisKey['ticket_tmp'], ticket, settime)

                    logging.info('[INFO] set ticket %s ' % ticket_res['ticket'])

            except Exception as e:
                print(datetime.datetime.now().strftime("%Y-%m-%d %H:%M:%S"), '[ERROR] get ticket. ', str(e))

        time.sleep(smtime)
    except Exception as e:
        print(datetime.datetime.now().strftime("%Y-%m-%d %H:%M:%S"), '[ERROR] redis err. ', str(e))
        exit()
```
### linux开机执行脚本
`crontab -e`可以查看定时任务，设定每天3点重启服务器
```
0 3 * * * /sbin/reboot
```
`vim /etc/rc.local`编辑开机任务
```
logdate="`date +%Y-%m-%d`"
# 获取微信token和ticket
nohup python3.6 /home/python/wechat_py/getWechatConfig.py > "/home/python/wechat_py/logs/"${logdate}".log" 2>&1 &
```