---
layout: post
title: X Window System
date: 2020-11-22
categories:
- Linux
---

X Window System 又称 X 或者 X11<br>
为其持续提供维护的组织: Xorg<br>
`架构`: Server/Client<br>
`客户端`: XServer<br>
XServer负责控制硬件, 绘制屏幕, 提供字体等工作;<br>
`服务器端`: XClient<br>
XClient负责处理XServer触发的事件(计算后给出响应);<br>
`特殊的XClient`: Window Manager<br>
一个XServer可以对接许多的XClient, 但是XClient之间却无法有效沟通, 所以需要一个特殊的XClient用来专门组织和管理其他所有的XClient, 这个特殊的XClient就是X Window Manager 也叫 WM<br>
常见的比较流行的WM: Gnome, Kde, Xfce...<br>
`提供登录界面的XClient`: Display Manager(DM)<br>
常见的DM: gnome display manager(gdm), lightdm...<br>
`启动WM的方法`:<br>
1. 命令行下`startx`启动
2. 在 display manager 的登录界面输入用户名密码进入wm
<br>
startx 其实是去调用执行了 xinit 这个程序<br>
`启动XServer的文件`: xserverrc<br>
位置: /etc/X11/xinit/xserverrc 或者 ~/.xserverrc<br>
`启动XClient的文件`: xinitrc<br>
位置: /etc/X11/xinit/xinitrc 或者 ~/.xinitrc<br>
XServer的默认端口号: 6000<br>
XServer的默认终端: tty7 对应0号<br>
`查看XServer的版本`: `sudo X -version`<br>
`提供XServer的字体库`: X Font Server(XFS)<br>

