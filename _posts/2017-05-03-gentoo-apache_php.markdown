---
title:  gentoo 配置 apache + php
layout: post
author:
name: macint0sh
---
首先确定 apache 版本:    

	#USE="apache2_modules_proxy apache2_modules_proxy_fcgi" emerge -av apache
	#USE="apache2 fpm " emerge -av php    

编辑 /etc/apache2/vhosts.d/00_default_vhost.conf:

	<VirtualHost *:80>
	ServerName localhost
	Include /etc/apache2/vhosts.d/default_vhost.include
	DocumentRoot /var/www/localhost/htdocs

	<Directory "/var/www/localhost/htdocs">
  	Options Indexes FollowSymLinks
  	AllowOverride All
  	Require all granted
    	#Satisfy Any ### apache 2.4 支持
	</Directory>

	<IfModule mpm_peruser_module>
	ServerEnvironment apache apache
	</IfModule>
	</VirtualHost>

## apache 2.2 版本
##### 编辑 /etc/php/fpm-php7.0/fpm.d/www.conf:

	user = apache
	group = apache
	listen = 127.0.0.1:9000


##### 编辑 /etc/apache2/modules.d/70_mod_php.conf:

	ProxyPassMatch ^/(.*\.php)$ fcgi://127.0.0.1:9000/var/www/localhost/htdocs/$1


##  apache 2.4 版本 
##### 编辑 /etc/php/fpm-php7.0/fpm.d/www.conf:

	user = apache
	group = apache
    
	;; listen = 127.0.0.1:9000  # 对应ProxyPassMatch方式
	listen = /var/run/php-fpm/www.sock # 对应SetHandler方式
    
	listen.owner = apache
	listen.group = apache
	listen.mode = 0660


##### 编辑 /etc/apache2/modules.d/70_mod_php.conf:

	## ProxyPassMatch ^/(.*\.php)$ fcgi://127.0.0.1:9000/var/www/localhost/htdocs/$1
	<FilesMatch "\.php$">
  		SetHandler "proxy:unix:/var/run/php-fpm/www.sock|fcgi://localhost"
	</FilesMatch>

注意: 70_mod_php.conf 文件属于 eselect-php 包
