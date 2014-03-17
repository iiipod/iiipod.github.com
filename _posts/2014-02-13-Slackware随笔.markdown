---
title: Slackware随笔
layout: post
author:
  name: macint0sh
---
 ===============================   
### 1. 基本系统安装:   
 A:        适度裁减无用软件   
 AP:      diffutils+nano+slackpkg   
 L:         ncurses   
 N:        dhcpcd+gnupg+net-tools+wget   
 ===============================   
### 2. 无线网络工具   
 N:wireless-tools   
 N:wpa_supplicant -> L:libnl3   
 ===============================   
### 3. 安装Xorg 显卡键盘鼠标驱动 blackbox xinit glibc 字体 xdm   
 ->xorg-server libgcrypt libpciaccess libgpg-error pixman libXfont libfontenc libXau libXdmcp   
 ->xf86-video-intel libdrm mesa   
 ->xf86-input-{keyboard,mouse,evdev} mtdev   
 ->xkeyboard-config   
 ->xkbcomp libX11 libxkbfile libxcb   
 ->blackbox fontconfig libXrender    
 ->xinit  -->>xauth libXmu    
 ->xinit  -->>rxvt libXpm  || ->xinit -->> xterm libXaw libXt libXpm   
 ->glibc   
 ->font-bitstream-type1   
 ->xdm xrdb libXinerama   
 ===============================    
### 4.安装flash插件    
下载 [flash player for linux](http://fpdownload.macromedia.com/get/flashplayer/pdc/11.2.202.346/install_flash_player_11_linux.i386.tar.gz)      
解压:
    `tar zxvf install\_flash\_player\_11\_linux.i386.tar.gz`       
    `cp libflashplayer.so /usr/lib/mozilla/plugins/`     
### 5.声音支持    
    slackpkg install alsa-utils alsa-lib    
 ===============================    


