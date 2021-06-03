---
layout: post
title: 用ansible管理windows的准备工作
date: 2021-06-03
categories:
- ansible
---

受控端操作系统以 windows server 2008R2 SP1 企业版为样例<br>
## 控制端
控制端Linux需要用pip安装一个python包: `pywinrm`<br>
## 受控端
1. 安装 `Microsoft .NET Framework 4.5`
* `注意顺序`: 要先安装.NET4.5后升级powershell, 并且重启机器
* 推荐使用离线安装包
2. 升级powershell到`4.0`以上版本
* windows server 2012及以上的版本默认powershell是4.0不需要升级
* `get-host`命令可以查看powershell的版本
* 2008R2装完机默认powershell是2.0版本
<img src="/assets/post_image/ansible_windows_1.png" width="80%" height="80%"><br>
* 升级完成并且重启后, get-host看到版本号为4.0
<img src="/assets/post_image/ansible_windows_2.png" width="80%" height="80%"><br>
3. 修改powershell的策略
```
# 查看策略
get-executionpolicy
# 把策略修改为remotesigned
set-executionpolicy remotesigned
```
4. 设置windows的远程管理
* 快速配置winrm service并启动该服务<br>
<img src="/assets/post_image/ansible_windows_3.png" width="80%" height="80%"><br>
* 查看监听状态<br>
<img src="/assets/post_image/ansible_windows_4.png" width="80%" height="80%"><br>
* 启用远程连接认证<br>
<img src="/assets/post_image/ansible_windows_5.png" width="80%" height="80%"><br>
* 设置加密方式为允许非加密<br>
<img src="/assets/post_image/ansible_windows_6.png" width="80%" height="80%"><br>
* 命令整理:<br>
```
winrm quickconfig
winrm enumerate winrm/config/listener
winrm set winrm/config/service/auth '@{Basic="true"}'
winrm set winrm/config/service '@{AllowUnencrypted="true"}'
```
5. windows防火墙配置
* 高级安全 Windows 防火墙
* 添加`入站`规则: 新建 \-\-\> 端口 \-\-\> TCP \-\-\> 特定: 5985
* 名称可以随便取(eg:Ansible)

## 小测试

