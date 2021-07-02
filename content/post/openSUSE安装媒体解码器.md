---
title: openSUSE
date: 2021-07-02T10:19:07+08:00
tags: [openSUSE]
---

### 解码器

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


### 系统备份

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
