---
layout: post
title: X Window System
date: 2020-11-22
categories:
- Linux
---

X Window System 又称 X 或者 X11<br>
为其持续提供维护的组织: Xorg<br>
架构: Server/Client<br>
客户端: XServer<br>
Xserver负责控制硬件, 绘制屏幕, 提供字体等工作;<br>
服务器端: XClient<br>
XClient负责处理Xserver触发的事件(计算后给出响应);<br>
特殊的XClient: Window Manager<br>
一个XServer可以对接许多的XClient, 但是XClient之间却无法有效沟通, 所以需要一个特殊的XClient用来专门组织和管理其他所有的XClient, 这个特殊的XClient就是X Window Manager 也叫 WM<br>
常见的比较流行的WM: Gnome, Kde, Xfce...<br>
提供登录界面的XClient: Display Manager(DM)<br>
常见的DM: gnome display manager(gdm), lightdm...<br>


