---
title: yum系列 postgresql + pip + jupyter安装配置
layout: post
author:
  name: macint0sh
---
#1. postgresql安装    
    
    #yum install postgresql-server postgresql-devel    

    #postgresql-setup initdb  // 初始化 //     

    #systemctl start postgresql // 启动 //    

    #su - postgres     

    $psql     

    postgres=# \password  // 首先修改postgres的主密码，防止无法登录 //      

    postgres=# create user <user_name> with password '<password>';     

    postgres=# create database <database_name>;    

    postgres=# grant all privileges on database <database_name> to <user_name>;       

    postgres=# \q    

    #systemctl restart postgresql // 重启 //    
    
    用户测试：    

    $psql -U <username> -d <database_name> -W [-h host_name] // 用户登录访问<database_name> //      

    登录失败：     

    检查/var/lib/pgsql/data/pg_hba.conf，将peer改成md5或者password模式     

    local all all md5    

    host  all all md5    

#2. pip安装    

    #yum install pip     

    注：软件仓库不带pip则需要手工下载安装    

    下载    
    wget https://bootstrap.pypa.io/get-pip.py    

    安装     
    #python get-pip.py      

#3. Jupyter安装     

    #yum install gcc gcc-c++ python-devel      

    #pip install jupyter      

    $jupyter-notebook运行，此时浏览器知道打开载入127.0.0.1:8888     

