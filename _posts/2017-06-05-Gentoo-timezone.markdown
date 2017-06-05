---
title: Gentoo时区设置
layout: post
author:
  name: macint0sh
---
#### 使用UTC
	#vi /etc/conf.d/hwclock 
	编辑clock="UTC"

#### 在/usr/share/zoneinfo/中查找可用的时区
	#ls /usr/share/zoneinfo
    
#### 假设要选择的时区是Asia/Shanghai，写进/etc/timezone文件：

	#echo "Asia/Shanghai" > /etc/timezone
    
#### 重新配置sys-libs/timezone-data包

	#emerge --config sys-libs/timezone-data

