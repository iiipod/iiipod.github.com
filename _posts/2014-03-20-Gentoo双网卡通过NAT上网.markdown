---
title:  Gentoo双网卡通过NAT上网
layout: post
author:
  name: macint0sh
---

# Gentoo 通过dhcpd分发IP NAT上网

### 1. 通过wlan0上网，eth0接路由器WAN口，在eth0配置dhcp服务
>编辑/etc/conf.d/net      

    ###eth0配置，ip地址可改变，保证与/etc/dhcp/dhcp.conf一致
    config_eth0="10.10.1.1/24"
    routes_eth0="10.10.1.0/24 via 10.10.1.1"
    modules="dhcpcd"
    dhcpcd_eth0="-t 10"
    dhcp_eth0="release nodns nontp nonis"
    ###wlan0配置,确保能上网即可   
    modules="wpa_supplicant"
    wpa_supplicant_wlan0="-Dnl80211"
    config_wlan0="dhcp"



>安装 net-misc/dhcp

    USE="server" emerge -av net-misc/dhcp

>编辑/etc/dhcp/dhcp.conf

    subnet 10.10.1.0 netmask 255.255.255.0 {
    # --- default gateway
    option routers                  10.10.1.1;
    option subnet-mask              255.255.255.0;
    option domain-name-servers      202.99.160.68,202.99.166.4; #中国联通DNS
    option time-offset              -18000; # Eastern Standard Time
    range dynamic-bootp 10.10.1.100 10.10.1.254;
    default-lease-time 21600;
    max-lease-time 43200;
    }
>打开内核的包转发功能

    echo 1 > /proc/sys/net/ipv4/ip_forward

>防火墙配置

    rc-service iptables save
    rc-service iptables start
    iptables -t nat -I POSTROUTING -s 10.10.1.0/24 -o eth0 -j MASQUERADE
    iptables -I FORWARD -i eth0 -j ACCEPT
    iptables -t nat -I POSTROUTING -s 10.10.1.0/24 -o wlan0 -j MASQUERADE
    iptables -I FORWARD -i wlan0 -j ACCEPT
>启动服务

    rc-service net.eth0 start
    rc-service net.wlan0 start
    rc-service dhcpd start
#===========================================
### 2. 通过eth0上网，wlan0作为无线路由器广播，在wlan0配置dhcp服务

>编辑 /etc/conf.d/net

    ###eth0配置，确保能上网即可
    config_eth0=( "192.168.1.10/24" )
    routes_ethn0="default via 192.168.0.1"
    ###wlan0配置，ip地址可改变，保证与/etc/dhcp/dhcp.conf一致
    config_wlan0="10.10.1.1/24"
    routes_wlan0="10.10.1.0/24 via 10.10.1.1"
    mode_wlan0="master"
>安装dhcp hostapd

    emerge -av net-misc/dhcp net-wireless/hostapd
>编辑/etc/dhcp/dhcp.conf

    subnet 10.10.1.0 netmask 255.255.255.0 {
    # --- default gateway
    option routers                  10.10.1.1;
    option subnet-mask              255.255.255.0;
    option domain-name-servers      202.99.160.68,202.99.166.4; #中国联通DNS
    option time-offset              -18000; # Eastern Standard Time
    range dynamic-bootp 10.10.1.100 10.10.1.254;
    default-lease-time 21600;
    max-lease-time 43200;
    }
>编辑/etc/hostapd/hostapd.conf

    interface=wlan0
    # 进程 PID 路径
    ctrl_interface=/var/run/hostapd
    # 无线网络的 SSID
    ssid=www
    # 无线网络的信道
    channel=6
    # 使用 WPA2 进行加密
    wpa=2
    # 无线网络密码
    wpa_passphrase=12345678
    # 无线配置
    # nl80211 网卡驱动
    driver=nl80211
    beacon_int=100
    # 802.11G 模式
    hw_mode=g
    # 禁用 802.11N
    ieee80211n=0
    # 使用 WPA-PSK 加密
    wpa_key_mgmt=WPA-PSK
    # 使用 CCMP 加密算法
    wpa_pairwise=CCMP
    max_num_sta=8
    wpa_group_rekey=86400
>打开内核的包转发功能

    echo 1 > /proc/sys/net/ipv4/ip_forward
>防火墙配置

    rc-service iptables save
    rc-service iptables start
    iptables -t nat -I POSTROUTING -s 10.10.1.0/24 -o eth0 -j MASQUERADE
    iptables -I FORWARD -i eth0 -j ACCEPT
    iptables -t nat -I POSTROUTING -s 10.10.1.0/24 -o wlan0 -j MASQUERADE
    iptables -I FORWARD -i wlan0 -j ACCEPT
>启动服务

    rc-service net.eth0 start
    rc-service net.wlan0 start
    rc-service hostapd start
    rc-service dhcpd start
>注意:启动hostapd时确保wpa_supplicant进程未占用wlan0口
#^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^
