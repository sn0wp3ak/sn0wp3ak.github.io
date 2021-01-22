---
title: Flask项目中实现JWT(双token)认证机制
date: 2020-04-06
categories:
- Flask
tags:
- JWT
---
## 核心思想
解决单一token的安全问题,设置2个token一个对接业务,有需求就带着业务token去认证然后操作相关功能,有效期较短,2小时;另一个token专门用来刷新业务token,简称刷新token,具有较长的过期时间,14天;
当业务token过期时,必须携带刷新token通过put接口来实现token的刷新,新的业务token还是存活2小时,这期间刷新token一直不变,直到14天后刷新token过期,重新开始新一轮的循环;在调用生成token的接口时,利用一个布尔型标识need_refresh_token来判断是否生成刷新token,保证有效期内只会生成新的业务token而刷新token则一直保持不变
## 准备工作
用Pycharm的朋友可以在你项目的Sources Root 目录下面的utils目录(如果有,没有可以新建)新建一个middleware.py文件用来存放各种中间件代码
* Sources Root 目录, 用来搜包的目录, 可以解决Pycharm层面的代码导包类报红提示
* 具体操作:右击文件夹-->Mark Directory As-->Sources Root

在工具文件夹utils下定义jwt的生成和校验的工具函数:
此处使用的是pyjwt三方包 ```$ pip install pyjwt```签名所用算法```HS256```
```python
import jwt
from flask import current_app


def generate_jwt(payload, expiry, secret=None):
    """
    生成jwt
    :param payload: dict 载荷
    :param expiry: datetime 有效期
    :param secret: 密钥
    :return: jwt
    """
    _payload = {'exp': expiry}
    _payload.update(payload)

    if not secret:
        secret = current_app.config['JWT_SECRET']

    token = jwt.encode(_payload, secret, algorithm='HS256')
    return token.decode()


def verify_jwt(token, secret=None):
    """
    检验jwt
    :param token: jwt
    :param secret: 密钥
    :return: dict: payload
    """
    if not secret:
        secret = current_app.config['JWT_SECRET']

    try:
        payload = jwt.decode(token, secret, algorithm=['HS256'])
    except jwt.PyJWTError:
        payload = None

    return payload
```
## 代码实现
1.middlewares.py中定义JWT的认证函数jwt_authentication
```python
from flask import g, request

from . import jwt_util

"""用户认证机制==>每次请求前获取并校验token"""

"@app.before_request 不使@调用装饰器 在 init文件直接装饰" 
def jwt_authentication():
    """
    1.获取请求头Authorization中的token
    2.判断是否以 Bearer开头
    3.使用jwt模块进行校验
    4.判断校验结果,成功就提取token中的载荷信息,赋值给g对象保存
    """
    auth = request.headers.get('Authorization')
    if auth and auth.startswith('Bearer '):
        "提取token 0-6 被Bearer和空格占用 取下标7以后的所有字符"
        token = auth[7:]
        "校验token"
        payload = jwt_util.verify_jwt(token)
        "判断token的校验结果"
        if payload:
            "获取载荷中的信息赋值给g对象"
            g.user_id = payload.get('user_id')
            g.refresh = payload.get('refresh')
```
2.在init文件中直接以调用钩子函数的方式装饰函数
```python
	"添加请求钩子"
    from utils.middlewares import jwt_authentication
    app.before_request(jwt_authentication)
```
3.在视图文件中定义token的生成方法
```python
def _generate_tokens(self, user_id, need_refresh_token=True):
	"""
	生成token 和refresh_token
	:param user_id: 用户id
	:return: token2小时, refresh_token14天
	"""
	pass
	'生成时间信息'
	current_time = datetime.utcnow()
	'指定有效期  业务token -- 2小时'
	expire_time = current_time + timedelta(hours=current_app.config['JWT_EXPIRY_HOURS'])

	'生成业务token  refresh 标识是否是刷新token''
	token = generate_jwt({'user_id': user_id, 'refresh': False}, expiry=expire_time)
	
	'给刷新token设置一个默认值None'
	refresh_token = None
	'根据传入的参数判断是否需要生成刷新token''
	'不需要生成的传入need_refresh_token=False,需要的传入True或不传使用默认值'
	if need_refresh_token:
	    '指定有效期  刷新token -- 14天'
	    refresh_expires = current_time + timedelta(days=current_app.config['JWT_REFRESH_DAYS'])
	    '生成刷新token'
	    refresh_token = generate_jwt({'user_id': user_id, 'refresh': True}, expiry=refresh_expires)
	'返回这两个token''
	return token, refresh_token
```
4.在视图文件中定义token的刷新方法
```python
def put(self):
        """
        1.携带刷新token,返回业务token
        2.用户必须登录g.user_id , 必须携带刷新token不允许携带业务token
        3.客户端请求参数 刷新token
        """
        '1.判断是否登录 2.判断refresh字段是否为True--是否是刷新token'
        if g.user_id and g.refresh is True:
            '调用生成token的方法 参数 need_refresh_token=False 不需要生成刷新token 仅生成业务token'
            token, refresh_token = self._generate_tokens(g.user_id, need_refresh_token=False)
            '返回业务token  修改成功状态码201'
            return {'token': token}, 201
        else:
            '失败返回错误信息和状态码'
            return {'message': 'Wrong Token'}, 403
        pass
```
5.强制登录装饰器,添加到视图类的method_decorators字典/列表中全局或局部装饰
```python
"""
登录验证装饰器
python中的装饰器：会改变被装饰器装饰的函数的函数名(属性)
使用方法：放在method_decorators中
"""
import functools


def login_required(f):
    '让装饰器装饰的函数属性不会变 -- name属性'
    '第1种方法,使用functools模块的wraps装饰内部函数'
    @functools.wraps(f)
    def wrapper(*args, **kwargs):
        if not g.user_id:
            return {'message': 'User must be authorized.'}, 401
        elif g.is_refresh_token:
            return {'message': 'Do not use refresh token.'}, 403
        else:
            return func(*args, **kwargs)
    '第2种方法,在返回内部函数之前,先修改wrapper的name属性'
    # wrapper.__name__ = f.__name__
    return wrapper
```
```python
from flask_restful import Resource


class AuthorizationResource(Resource):
    """
    认证
    """
    method_decorators = {
        'post': [login_required,set_db_to_write],
        'put': [login_required,set_db_to_read]
    }
```
