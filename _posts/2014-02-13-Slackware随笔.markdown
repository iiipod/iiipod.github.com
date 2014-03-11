---
title: Slackware随笔
layout: post
author:
  name: macint0sh
---
 ===============================<br>
### 1. 基本系统安装:<br>
 A:        适度裁减无用软件<br>
 AP:      diffutils+nano+slackpkg<br>
 L:         ncurses<br>
 N:        dhcpcd+gnupg+net-tools+wget<br>
 ===============================<br>
### 2. 无线网络工具<br>
 N:wireless-tools<br>
 N:wpa_supplicant -> L:libnl3<br>
 ===============================<br>
### 3. 安装Xorg 显卡键盘鼠标驱动 blackbox xinit glibc 字体 xdm<br>
 ->xorg-server libgcrypt libpciaccess libgpg-error pixman libXfont libfontenc libXau libXdmcp<br>
 ->xf86-video-intel libdrm mesa<br>
 ->xf86-input-{keyboard,mouse,evdev} mtdev<br>
 ->xkeyboard-config<br>
 ->xkbcomp libX11 libxkbfile libxcb<br>
 ->blackbox fontconfig libXrender<br>
 ->xinit  -->>xauth libXmu<br>
 ->xinit  -->>rxvt libXpm  || ->xinit -->> xterm libXaw libXt libXpm<br>
 ->glibc<br>
 ->font-bitstream-type1<br>
 ->xdm xrdb libXinerama<br>
 ===============================<br>

