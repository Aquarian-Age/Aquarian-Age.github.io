---
title: openSUSE
date: 2021-07-02T10:19:07+08:00
tags: [openSUSE]
toc: [true]
---

## 解码器

```bash

#!/bin/sh

echo "packman 软件源的媒体播放文件"
echo "添加packman源的国内镜像"
sudo zypper ar -f https://mirrors.ustc.edu.cn/packman/suse/openSUSE_Leap_15.0/ packman_ustc

set -v 

echo “开始安装....”
sudo zypper --no-refresh install libavutil55-3.4.4-lp150.9.1.x86_64 
sudo zypper --no-refresh install gstreamer-plugins-libav-1.12.5-lp150.3.2.x86_64
sudo zypper --no-refresh install libavcodec57-3.4.4-lp150.9.1.x86_64
sudo zypper --no-refresh install libavformat57-3.4.4-lp150.9.1.x86_64
sudo zypper --no-refresh install libswresample2-3.4.4-lp150.9.1.x86_64
sudo zypper --no-refresh install libavfilter6-3.4.4-lp150.9.1.x86_64
sudo zypper --no-refresh install libavresample3-3.4.4-lp150.9.1.x86_64
sudo zypper --no-refresh install libpostproc54-3.4.4-lp150.9.1.x86_64
sudo zypper --no-refresh install libswscale4-3.4.4-lp150.9.1.x86_64
sudo zypper --no-refresh install libavdevice57-3.4.4-lp150.9.1.x86_64
```

## 录屏
图片编辑
```bash
sudo zypper in -y kolourpaint simplescreenrecorder
```

## 系统备份

rsync-backup-system.sh
```bash
#!/bin/bash

echo " " >> /home/xxxx/logs/rsync-system-backup-info.log
echo "**********$(date)开始备份**********" >> /home/xxxx/logs/rsync-system-backup-info.log 2>&1

### -v參數去掉 避免在/var/spool/mail/root中輸出和log file重複信息
# -a 存档模式 等于-rlptgoD(没有-H，-A，-X); 
# -X 保留扩展属性;
# -A 保留ACL（暗示--perms）;
# -z 在传输过程中压缩文件数据
# -p 保留权限
# -t 保留修改时间
# -g 保留组
# -o 保留所有者（仅限超级用户）
# -r 递归目录
# -l 将符号链接复制为符号链接
# -H 保留硬链接
# -z 传输过程压缩
# -R 使用相对路径名称(从源目录的起始位置 保留路径 备份到目标目录)

sudo rsync -aptgovrlHAX --delete-excluded --partial --log-file=/home/xxxx/logs/rsync-system-backup.log / /mnt/sdcdata/system-backup/ --exclude={"/usr/src/*","/media/*","/sys/*","/proc/*","/mnt/*","/tmp/*","/run/*","/dev/*","/home/*","/var/tmp/*","/var/run/*","/var/log/*","/var/adm/*","/var/cache/*","/usr/share/doc/*"} && echo "**********$(date):系统备份完毕**********" >> /home/xxxx/logs/rsync-system-backup-info.log 2>&1 
```

## 远程备份

```bash
#!/bin/sh
rsync -aptgovrlHAXzP -e "ssh -p passwd" --delete-excluded --exclude={"/media/*","/sys/*","/proc/*","/mnt/*","/tmp/*","/home/*","/var/run/*","/var/tmp/*"} root@AA:AA:AA:AA:/ /run/media/AAA/data/rsync-vps-backup/
```

## DNS静态解析

/etc/sysconfig/network/config

把 NETCONFIG_DNS_POLICY="auto" 改为 NETCONFIG_DNS_POLICY=""

更改 resolv.conf 添加如下内容

nameserver 223.5.5.5

## 禁用KDE的baloo文件索引功能

```bash
balooctl disable
```

## 禁用akonadi服务

~/.config/akonadi
```bash
StartServer=false
```

## 修复grub方法
挂载顺序里面 / 在最前面
```bash
#!/bin/sh
set -v
mount /dev/sda4 /mnt
mount /dev/sda2 /mnt/boot/efi
mount /dev/sdb3 /mnt/var
mount /dev/sdb4 /mnt/opt
mount /dev/sdb5 /mnt/home
mount -t proc proc /mnt/proc
mount --rbind /sys /mnt/sys
mount --rbind /dev /mnt/dev
chroot /mnt /bin/bash
```

重新安装grub
```bash
grub2-mkconfig -o /boot/grub2/grub.cfg
grub2-install /dev/sda
```

## proxychain4

用proxychain4实现firefox浏览器去插件全局走代理的方法
```bash
sudo echo "socks5 127.0.0.1 1080" >> /etc/proxychain4.conf
```


## sudo免密输入 


```bash
sudo -i
chmod u+w /etc/sudoers
```

编辑　/etc/sudoers
```text
%wheel ALL=(ALL) ALL 取消前面的注释
```

设置免密命令
```
username ALL=(ALL) NOPASSWD:/usr/bin/zypper,/usr/sbin/privoxy,/usr/bin/rsync,/usr/sbin/cryptsetup,/usr/sbin/hdparm,/usr/bin/rpmbuild,/usr/bin/umount,/usr/bin/mount
```

添加用户到wheel组
```
usermod -a -G wheel username
```

还原文件属性
```bash
chmod u-w /etc/sudoers
visudo -c -f /etc/sudoers
```


## 联网

查看无线网卡的设备名

ip a

查看可用的NetworkManager连接

su
nmcli con show
```bash
nmcli con show
NAME                            UUID                                  TYPE      DEVICE 
1101                            909fe38a-b8e2-4b68-9c1c-431fae269514  wifi      wlan0  
Wired connection 1              2fbd9c16-aa4c-4885-a1bb-c6bd5d252d92  ethernet  eth0      
```

之前有过的连接　无线连网的命令就是
```bash
nmcli con up uuid 909fe38a-b8e2-4b68-9c1c-431fae269514
```

要是没有连接 新建无线连接用这个命令
```bash
nmcli dev wifi connect <SSID> password <password>
```

新建有线连接用这个命令
```bash
nmcli con add type ethernet con-name <SSID> ifname enp3s0
```

要是静态连接 在上面的命令后面添加
```bash
ip4 192.168.1.50/24 gw4 192.168.1.1
```

连接创建好后 假如要修改 dns
```bash
nmcli con mod <SSID> ipv4.dns “8.8.8.8 8.8.4.4”
```

然后 nmcli con down <SSID> 再重新 up 一下就可以



## 添加开机启动服务

/etc/systemd/system/rc-local.service 
```text 
[Unit]  
Description=/etc/rc.local Compatibility  
ConditionFileIsExecutable=/etc/rc.local  
After=network.target  

[Service]  
Type=forking  
PIDFile=/var/run/rc.local.pid
ExecStart=/etc/rc.local start  
TimeoutSec=5  
RemainAfterExit=yes  
GuessMainPID=yes 

[Install]  
WantedBy=multi-user.target  
```

systemctl enable rc-local.service

/etc/rc.local
```bash
#!/bin/sh
echo "$(date): 测试rc.local...." >> /var/log/test-rc.log
```
chmod +x /etc/rc.local
