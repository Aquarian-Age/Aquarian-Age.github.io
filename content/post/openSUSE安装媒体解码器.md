---
title: "OpenSUSE安装媒体解码器"
date: 2021-07-02T10:19:07+08:00
draft: true
tags: [openSUSE]
---

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
