---
title:  Gentoo配置openvpn服务
layout: post
author:
  name: macint0sh
---
#   Gentoo配置openvpn服务
\# cat /usr/src/linux/.config | grep CONFIG_TUN  

服务端配置:
    emerge openvpn easy-rsa
认证:
    
客户端配置:       

 Gentoo >>>
    # cat /usr/src/linux/.config | grep CONFIG_TUN      ** 检查内核是否支持tun **
    # emerge openvpn
    # ln -s /etc/init.d/openvpn /etc/init.d/openvpn.ok
    # rc-update add openvpn.ok default
    
    # mkdir -p /etc/openvpn/ok
    # cp ca.crt client1.crt client1.key /etc/openvpn/ok/
编辑配置文件
\# vi /etc/openvpn/ok.conf
    resolv-retry infinite
    ns-cert-type server
    redirect-gateway
    keepalive 20 60
    #tls-auth ta.key 1
    comp-lzo
    verb 3
    mute 20
    route-method exe
    route-delay 2
    ####################
    # specify client-side   这个client不是自定义名称 不能更改
    client

    # tun/tap devcie    要与前面server.conf中的配置一致
    dev tun

    # protocol, according to server     要与前面server.conf中的配置一致
    proto udp
    
    # server address        将11.22.33.44替换为你VPS的IP，端口与server.conf中配置一致。
    remote 11.22.33.44 8080

    # connection
    comp-lzo
    resolv-retry 30
    nobind

    # persistent device and keys
    persist-key
    persist-tun

    # keys settings
    ca ok/ca.crt
    cert ok/client.crt
    key ok/client.key

    # optional tls-auth
    # tls-auth exmaple/ta.key 1

    # logging
    log /etc/openvpn/openvpn.log
    verb 4
    
