---
title: python模块flask示例
date: 2019-06-27 16:05:01
tags:
    - python
---
python模块flask接口，实现返回微信公众号的access_token和ticket以及微信模板消息发送。
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
    'send_temp_msg': 'https://api.weixin.qq.com/cgi-bin/message/template/send?access_token=%s',  # 发送模板消息
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

logfilename = '/home/python/pyapis/logs/' + str(time.strftime('%Y%m%d')) + '.log'
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
# @Date    : 2018-06-19 23:55:52
# @Author  : Your Name (you@example.org)
# @Link    : link
# @Version : 1.0.0

from flask import Flask
from flask_restful import reqparse, abort, Api, Resource
from AppLoggin import logging
from AppConfig import myRedis, redisKey, weChat_api, myWeChat
import redis
import requests
import json


app = Flask(__name__)
api = Api(app)

pool = redis.ConnectionPool(**myRedis)
r = redis.Redis(connection_pool=pool)

# 空方法
class Emptys(Resource):
    def get(self):
        return {'errcode': 404, 'errmsg': '页面不存在'}

    def post(self):
        return {'errcode': 404, 'errmsg': '页面不存在'}


# 获取微信token
class AccessToken(Resource):
    def get(self):
        access_token = r.get(redisKey['access_token'])
        return {'errcode': 0, 'errmsg': '获取成功', 'value': access_token.decode('utf-8') if access_token else ''}


# 获取微信ticket
class Ticket(Resource):
    def get(self):
        ticket = r.get(redisKey['ticket'])
        return {'errcode': 0, 'errmsg': '获取成功', 'value': ticket.decode('utf-8') if ticket else ''}

# 发送模板消息
class SendTemplateMessage(Resource):
    def post(self):
        url = 'http://127.0.0.1:5000/access_token'
        res = requests.get(url).json()
        access_token = res['value']
        if access_token:
            send_url = weChat_api['send_temp_msg'] % access_token

            parser = reqparse.RequestParser()

            parser.add_argument('openid', type=str)
            parser.add_argument('template_id', type=str)
            parser.add_argument('url', type=str)
            parser.add_argument('first_val', type=str)
            parser.add_argument('keyword1_val', type=str)
            parser.add_argument('keyword2_val', type=str)
            parser.add_argument('remark_val', type=str)

            args = parser.parse_args()
            if not args['openid']:
                return {'errcode': 2, 'errmsg': 'openid不能为空'}

            if not args['template_id']:
                return {'errcode': 2, 'errmsg': 'template_id不能为空'}

            if not args['first_val'] or not args['keyword1_val'] or not args['keyword2_val'] or not args['remark_val']:
                return {'errcode': 2, 'errmsg': '模板消息不能为空'}

            temp_data = {
                'touser': args['openid'],
                'template_id': args['template_id'],
                'url': args['url'],
                'data': {
                    'first': {
                        'value': args['first_val'],
                    },
                    'keyword1': {
                        'value': args['keyword1_val'],
                    },
                    'keyword2': {
                        'value': args['keyword2_val'],
                    },
                    'remark': {
                        'value': args['remark_val'],
                    }
                }
            }

            send_result = requests.post(send_url, verify=False, json=temp_data)
            res = send_result.json()

            logging.info('[data] %s ,res %s' % (json.dumps(temp_data, ensure_ascii=False), send_result.text))

            return {'errcode': res['errcode'], 'errmsg':  res['errmsg']}
        else:
            return {'errcode': 1, 'errmsg': 'access_token获取失败'}


api.add_resource(Emptys, '/')
api.add_resource(AccessToken, '/access_token')
api.add_resource(Ticket, '/ticket')
api.add_resource(SendTemplateMessage, '/send_template_message')

if __name__ == '__main__':
    # app.config['JSON_AS_ASCII'] = False
    app.config.update(RESTFUL_JSON=dict(ensure_ascii=False))
    app.run(host='127.0.0.1', port='5000', debug=True)
```
### linux开机执行脚本
`crontab -e`可以查看定时任务，设定每天3点重启服务器
```
0 3 * * * /sbin/reboot
```
`vim /etc/rc.local`编辑开机任务
```
# 启动python的api接口
logdate="`date +%Y-%m-%d`"
nohup python3.6 /home/python/pyapis/Main.py > "/home/python/pyapis/logs/"${logdate}".log" 2>&1 &
```