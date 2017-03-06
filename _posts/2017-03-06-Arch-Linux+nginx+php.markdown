---
title: Arch Linux 配置 nginx + php
layout: post
author:
  name: macint0sh
---
# Arch Linux 配置 nginx + php

	#pacman -S nginx php

###配置nginx，修改配置文件，主要是改成以下内容

	listen 80;
    server_name www.x.cn;
	location / {
        root   /srv/web;
        index  index.php index.html index.htm;
    }

    location ~ \.php$ {
        fastcgi_pass   127.0.0.1:9000;
        fastcgi_index  index.php;
        fastcgi_param  SCRIPT_FILENAME  /srv/web$fastcgi_script_name;
        include        fastcgi_params;
    }

 ####修改 /etc/php-fpm.d/www.conf配置

	user  = www
	group = www
	listen = 127.0.0.1:9000



