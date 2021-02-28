---
layout: post
title: Linux中的DHCP服务
date: 2020-07-18
categories:
- linux
tags:
- dhcp
---

DHCP动态主机配置协议
* 基于UDP协议
* 仅限于局域网内部使用
* 主要用于自动分配IP地址等网络参数

### 术语
作用域: 一个完整的IP地址段<br>
超级作用域: 管理处于一个物理网络中的多个逻辑子网段<br>
排除范围(保留地址): 保留一些IP地址, 不会给DHCP客户端分配<br>
租约: DHCP客户端使用分配到的IP的时间<br>
预约: 使特定设备总是占有某个特定的IP地址<br>

Linux操作系统中常用的DHCP服务程序: `dhcpd`<br>
> yum install dhcp

配置文件: `/etc/dhcp/dhcpd.conf`<br>
示例配置文件: `/usr/share/doc/dhcp*/dhcpd.conf.example`<br>

配置文件的组成架构:
* 全局配置参数
* 子网段声明
* 地址配置选项
* 地址配置参数

dhcpd常用配置文件参数:<br>

| 参数                          | 作用                               |
| :-                            | :-                                 |
| ddns-update-style             | 定义DNS服务动态更新的类型          |
|                               | none 不支持动态更新                |
|                               | interim 互动式更新                 |
|                               | ad-hoc 特殊更新                    |
| [allow/ignore] client-updates | 允许/忽略客户端更新DNS记录         |
| default-lease-time            | 默认超时时间                       |
| max-lease-time                | 最大超时时间                       |
| option domain-name-servers    | 定义DNS服务器地址                  |
| option domain-name            | 定义DNS域名                        |
| range                         | 定义可以用于分配的IP地址池         |
| option subnet-mask            | 定义客户端的子网掩码               |
| option routers                | 定义客户端的网关地址               |
| broadcase-address             | 定义客户端的广播地址               |
| ntp-server                    | 定义客户端的网络时间服务器NTP      |
| nis-servers                   | 定义客户端的NIS域服务器的地址      |
| Hardware                      | 指定网卡接口的类型与MAC地址        |
| server-name                   | 向DHCP客户端通知DHCP服务器的主机名 |
| fixed-address                 | 将某个固定的IP地址分配给制定主机   |
| time-offset                   | 指定客户端与格林尼治时间的偏移差   |

在配置完DHCP服务后, 需要重启服务并加入开机启动项<br>
```
systemctl restart dhcpd
systemctl enable dhcpd
```

### 分配固定IP地址

IP和MAC地址绑定<br>
```
host 主机名 {
	hardware ethernet MAC地址;
	fixed-address 保留的IP地址;
}
```
重启DHCP服务
```
systemctl restart dhcpd
```





