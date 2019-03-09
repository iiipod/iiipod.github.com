---
title:  gentoo 配置 nginx + php
layout: post
author:
name: macint0sh
---
首先安装 nginx:    

	# emerge -av nginx    

编辑 /etc/nginx/nginx.conf:

    user apache apache;    #使用apache用户可以兼容apache的php设置    

    events {    
    	#worker_connections 1024;    
    	#use epoll;    
    }    

    http {    
    	include /etc/nginx/mime.types;    

    	server {    
	    	listen 80;    
		    server_name localhost;    
		    #autoindex on;    

		    location / {    
		    root /z/backup/web; #设置网站根目录    
		    index index.php index.html;    
		    }    

		    location ~ \.php$ {    
		    fastcgi_pass	unix:/var/run/php-fpm/www.sock; #与www.conf配置一致    
            #fastcgi_pass   127.0.0.1:9000; #与www.conf配置一致    
		    fastcgi_index index.php;    
		    fastcgi_param SCRIPT_FILENAME /z/backup/web$fastcgi_script_name;    
		    include fastcgi_params;    
		    }    
	    }    
    }    

安装php:    

    #USE="fpm" emerge -av php    

修改 /etc/php-fpm.d/www.conf配置:    

    user = apache    #使用apache用户兼容apache    
    group = apache   #使用apache组兼容apache    

    ;listen = 127.0.0.1:9000  # 对应ProxyPassMatch方式    
    listen = /var/run/php-fpm/www.sock # 对应SetHandler方式     

