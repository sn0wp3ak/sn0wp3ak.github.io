---
layout: post
title: VMware NAT模式的连通性问题
date: 2021-06-17
categories:
- virtualization
---
## 问题描述
宿主机ping不通虚拟机, 或者, 虚拟机ping不通宿主机<br>
## 问题原因
网络配置问题<br>
## 解决方法
### 1.理解VMware的NAT拓扑结构<br>
<img src="/assets/post_image/vmware_nat.png">
### 2.明确需要修改的地方<br>
大致上需要修改四处:<br>

* 第一处: VMnet8-NAT的网关IP地址(即虚拟交换机的IP地址)
	* 在VMware中编辑菜单下的虚拟网络编辑器里面
	* 选择VMnet8 NAT设置 注意需要设置`2处`: 网段和网关IP
	<img src="/assets/post_image/vmware_nat_1.png" width="60%" height="60%"><br>
	<img src="/assets/post_image/vmware_nat_2.png" width="60%" height="60%"><br>
* 第二处: 虚拟机操作系统中设置静态IP和网关地址
	* 此处设置的网关地址和虚拟交换机设置的需要`一样`
	* 虚拟机的IP地址和虚拟交换机需要在同一个网段
* 第三处: 虚拟网卡VMnet8的IP地址
	* 在宿主机网络适配器选项中将VMnet8的IP地址也设置为和虚拟交换机`同一网段`的地址
	* 举例: 虚拟交换机192.168.2.2 / 虚拟网卡192.168.2.1
	<img src="/assets/post_image/vmware_nat_3.png" width="60%" height="60%"><br>
* 第四处: 关闭虚拟机防火墙, 开启宿主机的入站策略
	* Windows10最好把防火墙的`文件和打印机共享`开启
	* Linux虚拟机关闭firewalld, Window虚拟机直接关闭防火墙
	<img src="/assets/post_image/vmware_nat_4.png" width="60%" height="60%"><br>
	<img src="/assets/post_image/vmware_nat_5.png" width="60%" height="60%"><br>

### 3.注意事项<br>
虚拟机配置完网络后, 重启服务或者直接重启虚拟机<br>
配置完毕后虚拟机ping不通外网, 注意是域名解析的问题, 在`/etc/resolv.conf`中添加几个`nameserver`, 比如8.8.8.8和114.114.114.114等域名解析服务器<br>


