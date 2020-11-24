---
title: ArchLinux安装记录
date: 2020-11-10
categories:
- Linux
tags:
- ArchLinux 
---

# 准备工作

>1. 烧录一个启动U盘<br/>
>2. 把Bios调成UEFI启动

# 正式安装
1. 检查启动模式<br/>
	`ls /sys/firmware/efi/efivars`<br/>
	如果没有efi目录则说明U盘系统不是用UEFI启动的<br/>
	现在的笔记本只要不是太次的都用UEFI启动了
2. 修改终端字体<br/>
	默认的终端字体太小, 需要改一下字体方便安装;<br/>
	推荐设置字体 `setfont /usr/share/kbd/consolefonts/LatGrkCyr-12x22.psfu.gz`
3. 联网
	- 看下网络设备(网卡) `ip link`
	- 启动(up)网卡 `ip link set 网卡名 up`
	- 我一般用无线网， 扫描一下附近的wifi `iwlist 无线网卡名 scan | grep ESSID`
	- 生成wifi配置文件 `wpa_passphrase wifi名 密码 > internet.conf`
	- 连接无线网 `wpa_supplicant -c internet.conf -i 无线网卡名 &`
	- 动态分配IP地址 `dhcpcd &`
	- 测试联通性 `ping baidu.com`<br/>

	**建议把联网步骤写入shell脚本，添加到开机启动配置中**<br/>
4. 更新系统时间
	- `timedatectl set-ntp true`
5. 分区(重点)
	- 先看下存储设备列表 `fdisk -l`
	- 根据提示分区，UEFI启动官方要求要分一个启动分区!!!
	- 我的常用分区，固态512M给/boot分区，16G给swap(内存16G)，剩下全给根目录/; 机械就默认一个分区用来给/home
	- 格式化分区 /boot 格式化成Fat32 `mkfs.fat -F32 /dev/...p1`
	- 格式化swap分区 `mkswap /dev/...p3`
	- 格式化根和家分区 `mkfs.ext4 /dev/...p2` 和 `mkfs.ext4 /dev/sda1`
6. 激活swap分区 
	- `swapon /dev/...p3`
7. 挂载各分区
	- 先把根分区挂载到U盘系统的/mnt目录下 `mount /dev/...p2 /mnt`
	- 在/mnt目录下创建boot和home目录 `mkdir /mnt/boot` 和 `mkdir /mnt/home`
	- 分别吧挂载home和boot分区 `mount /dev/...p1 /mnt/boot` 和 `mount /dev/sda1 /mnt/home`
8. 确定国内软件源在列表最上边
	- 拉取软件时添加颜色信息，`vim /etc/pacman.conf`
	- 修改镜像源的顺序 `vim /etc/pacman.d/mirrorlist`
9. 基础安装
	- 安装内核和驱动 `pacstrap /mnt base linux linux-firmware`
	- `genfstab -U /mnt >> /mnt/etc/fstab`
10. 进入安装好的系统
	- `arch-chroot /mnt` 进入挂载点所在的系统环境
11. 时区设置
	- `ln -sf /usr/share/zoneinfo/Asia/Shanghai /etc/localtime`
	- 同步系统时间 `hwclock --systohc`
12. 本地化
	- `exit` 先退出挂载点的系统环境
	- `vim /mnt/etc/locale.gen` 开启对应注释(en_US.UTF-8)
	- 再次进入挂载点的系统环境 `arch-chroot /mnt`
	- 生产本地化 `locale-gen`
	- 设置语言 `vim /mnt/etc/locale.conf` `LANG = en_US.UTF-8`
13. 修改hostname
	- `vim /mnt/etc/hostname` 写入计算机名
14. 修改hosts
	- `vim /mnt/etc/hosts`
	- 127.0.0.1	localhost
	- \:\:1		localhost
	- 127.0.0.1	hostname.localdomain hostname
15. 修改root密码
	- 进入挂载系统环境 `passwd 密码`
16. 配置grub
	- 安装grub `pacman -S grub efibootmgr intel-ucode os-prober`
	- `mkdir /boot/grub` `grub-mkconfig /boot/grub/grub.cfg`
	- `grub -install --target=x86_64-efi --efi-directory=/boot`
17. 安装一些软件
	- 提前安装一些进入系统后必备的软件
	- `pacman -S vim wpa_supplicant dhcpcd`
<br/>
**至此archlinux已经安装完毕， reboot通过grub界面进入系统**<br/>

# 安装完成后需要做的事

1. 设置字体
	- `setfont`
2. 联网
3. 更新一下
	- `pacman -Syyu`
4. 安装一些基础命令(重要)
	- `pacman -S base-devel`
5. 安装手册
	- `pacman -S man`
6. 添加普通用户
	- `useradd -G wheel -m 用户名` 添加到wheel组
	- `ln -s /usr/bin/vim /usr/bin/vi`
	- 编辑 `visudo` 打开wheel组的注释
7. 安装图像界面(重要)
	- 先安装X服务 `sudo pacman -S xorg xorg-server`
	- 选择一个喜欢的桌面环境安装, 本人使用Mate
	- 安装登录管理器 `sudo pacman -S lightdm lightdm-gtk-greeter`
	- 开机启动登录管理器 `systemctl enable lightdm`
8. 推荐安装
	- AUR 推荐yay 从github拉取 `makepkg -si`
	- 谷歌Chrome浏览器, `yay -S google-chrome`
	- 挂载NTFS格式的设备 `pacman -S ntfs-3g`


