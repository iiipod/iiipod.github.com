---
title: RaspberryPi+distccd交叉编译 
layout: post
author:
  name: macint0sh
---
RaspberryPi+distccd交叉编译
# Raspberry+Server[s]：
    #emerge -lv distcc
    #nano /etc/conf.d/distccd           //编辑distcc配置文件//
        {
            ...
            DISTCCD_OPTS="${DISTCCD_OPTS} --allow 192.168.1.0/24"
            ...
        }
    #rc-update add distccd default      //添加distccd到默认运行级别//
    #rc-config start distccd            //启动distccd//
    =========================================================================
    
Raspberry：

    #nano /etc/make.conf
        {
            ...
            FEATURES="distcc"
            ...
        }
        
    #cd /usr/lib/distcc/bin 
    #ls -l
        {    
        total 0
        c++ -> /usr/bin/distcc
        cc -> /usr/bin/distcc
        g++ -> /usr/bin/distcc
        gcc -> /usr/bin/distcc
        armv6j-hardfloat-linux-gnueabi-c++ -> /usr/bin/distcc
        armv6j-hardfloat-linux-gnueabi-g++ -> /usr/bin/distcc
        armv6j-hardfloat-linux-gnueabi-gcc -> /usr/bin/distcc
        }
        
    #nano /usr/lib/distcc/bin/armv6j-hardfloat-linux-gnueabi-wrapper
        {
            #!/bin/bash
            exec /usr/lib/distcc/bin/armv6j-hardfloat-linux-gnueabi-g${0:$[-2]} "$@"
        }
    
    #rm c++ g++ gcc cc 
    #chmod a+x /usr/lib/distcc/bin/armv6j-hardfloat-linux-gnueabi-wrapper
    #ln -s armv6j-hardfloat-linux-gnueabi-wrapper cc
    #ln -s armv6j-hardfloat-linux-gnueabi-wrapper gcc
    #ln -s armv6j-hardfloat-linux-gnueabi-wrapper g++
    #ln -s armv6j-hardfloat-linux-gnueabi-wrapper c++
    
    #ls -l
        {
        total 4
        armv6j-hardfloat-linux-gnueabi-c++ -> /usr/bin/distcc
        armv6j-hardfloat-linux-gnueabi-g++ -> /usr/bin/distcc
        armv6j-hardfloat-linux-gnueabi-gcc -> /usr/bin/distcc
        armv6j-hardfloat-linux-gnueabi-wrapper
        c++ -> armv6j-hardfloat-linux-gnueabi-wrapper
        cc -> armv6j-hardfloat-linux-gnueabi-wrapper
        g++ -> armv6j-hardfloat-linux-gnueabi-wrapper
        gcc -> armv6j-hardfloat-linux-gnueabi-wrapper
        }
        
    #nano /usr/sbin/distcc-watch
        {
            #!/bin/bash
            # This script will only print non-blank results from distccmon-text N=1 # seconds to sleep in between checks
            while true; do
            task=`DISTCC_DIR="/var/tmp/portage/.distcc/" distccmon-text`
            taskshort=`echo $task | tr -d ' '`
            len=${#taskshort}
            
            if [ $len -ne 0 ];then 
                echo "$task"
            fi

            sleep $N
            done
        }
        
    #watch -n1 DISTCC_DIR="/var/tmp/portage/.distcc/" distccmon-text
    
Server[s]：
    
    #emerge -lv crossdev
    #cd ~/bin
    #nano con-profile2files.sh
        {
            #!/bin/bash
            PROFILE_DIR="/etc/portage"

            if [ ! -e ${PROFILE_DIR} ]; then
                mkdir ${PROFILE_DIR};
            fi;

            for PACK_DIR in package.keywords package.use package.unmask package.mask; do
            CUR_DIR="${PROFILE_DIR}/${PACK_DIR}"
            if [ ! -e ${CUR_DIR} ]; then
                mkdir ${CUR_DIR}
            fi

            if [ -e ${CUR_DIR} -a ! -d ${CUR_DIR} ]; then
                mv ${CUR_DIR} ${CUR_DIR}.moving
                mkdir ${CUR_DIR}
                mv ${CUR_DIR}.moving ${CUR_DIR}/monolithic
            fi
            done

            echo "Completed!"
        }
    #crossdev -S -v -t armv6j-hardfloat-linux-gnueabi 
    #=*=#CFLAGS="-O2 -pipe" CXXFLAGS="${CFLAGS}" crossdev -S -v -t armv6j-hardfloat-linux-gnueabi //上述出错时使用//
