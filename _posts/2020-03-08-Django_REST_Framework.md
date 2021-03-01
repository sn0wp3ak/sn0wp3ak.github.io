---
layout: post
title: Django REST Framework
date: 2020-03-08
categories:
- django
---
### django rest framework的作用:
* 封装了序列化和反序列的过程,简化了代码,提高开发效率

### 特点
* 提供了定义序列化器Serializer的方法，可以快速根据 Django ORM 或者其它库自动序列化/反序列化；
* 提供了丰富的类视图、Mixin扩展类，简化视图的编写；
* 丰富的定制层级：函数视图、类视图、视图集合到自动生成 API
* 限流系统
* 支持多种身份认证和权限认证方式
* 直观的 API web 界面 测试接口
* 可扩展性高，插件丰富

### 安装
```
pip install djangorestframework
```

### 注册应用
```python
INSTALLED_APPS = [
    ...
    'rest_framework',
]
```

