---
title:  Gentoo双网卡通过NAT上网
layout: post
author:
  name: macint0sh
---

# Gentoo 通过dhcpd分发IP NAT上网

#### 1. 通过wlan0上网，eth0接路由器WAN口，在eth0配置dhcp服务
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
    iptables -t nat -A POSTROUTING -s 10.10.1.0/24 -o eth0 -j MASQUERADE
    iptables -A FORWARD -i eth0 -j ACCEPT
    iptables -t nat -A POSTROUTING -s 10.10.1.0/24 -o wlan0 -j MASQUERADE
    iptables -A FORWARD -i wlan0 -j ACCEPT
#==============================
