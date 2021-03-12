---
layout: post
title: Python数据库编程 -- MySQL篇
date: 2020-04-06
categories:
- python
tags:
- mysql
---

### PyMySQL的使用
导包:
```python
import pymysql
```

使用流程:
```
1.建立连接
2.通过连接获取游标
3.使用游标执行SQL语句
4.查询结果
5.关闭游标关闭连接
```

1. 建立连接(创建连接对象)
```python
con = pymysql.connect(host="MySQL服务器IP地址", port="端口号", 
user="用户名", password="密码", database="使用的数据库")
```
2. 获取游标对象
```python
cursor = con.cursor()
```
3. 游标执行SQL
```python
sql = "SELECT * FROM 表;"
cursor.execute(sql)  # 返回值是被SQL影响的行数
```
4. 查询结果
```python
data = cursor.fetchall()  # 一次性获取所有, 空数据返回()
data = cursor.fetchone()  # 一次性获取一行, 空数据返回None
```
5. 关闭游标和连接
```python
cursor.close()
con.colse()
```

提交修改: `con.commit()`<br>
回滚数据: `con.rollback()`<br>

### 防止SQL注入
用户提交有恶意的数据与SQL语句进行字符串方式的拼接, 影响了SQL语句的语义, 最终导致数据泄露<br>

产生原因: SQL语句中参数用"%s"占位后, 格式化输出%后面拼接的字符串带有注释的效果, 比如`#`<br>

解决方法:
1. 占位符不加引号, 只是用%s进行占位
2. 构造参数列表

```sql
cursor.execute(" ... %s", [arg1, arg2, ...])
```

所以说在构造execute方法的SQL语句时, 不要使用带引号的%s进行占位<br>

