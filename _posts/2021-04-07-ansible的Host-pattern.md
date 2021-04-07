---
layout: post
title: ansible的Host-pattern
date: 2021-04-07
categories:
- ansible
---

host-pattern用于匹配需要被管理的主机<br>

* `all` 表示清单inventory中的所有主机
* 通配符
  * `"*"` 匹配所有
  * 通配符`192.168.0.*`匹配清单中属于该网段的所有主机
* 指定组 `"websrvs"`
* 逻辑或 `"websrvs:appsrvs"`
* 逻辑与 `"websrvs:&dbsrvs"`
* 逻辑非 `"websrvs:!dbsrvs"`
* 混合逻辑 `"websrvs:dbsrvs:&appsrvs:!ftpsrvs"`
* 正则表达式 `"~(web|db).*\.baidu\.com"`

### 清单

清单路径: `/etc/ansible/hosts`

清单里的条目要么用`IP地址`, 要么用`FQDNs`<br>



