---
title: Using clang in Gentoo
layout: post
author:
  name: macint0sh
---
Using clang in Gentoo

###  \#emerge llvm clang dragonegg
###  \#vi   /etc/portage/env/clang
    CC=clang
    CXX=clang++
    CFLAGS="-O2 -march=native -mtune=native -pipe"
    CXXFLAGS="-O2 -march=native -mtune=native -pipe"
    LDFLAGS="-Wl,--as-needed"
###  \#vi   /etc/portage/env/gcc
    CC="gcc"
    CXX="g++"
    CFLAGS="-O2 -march=native -mtune=native -pipe"
    CXXFLAGS="-O2 -march=native -mtune=native -pipe"
    LDFLAGS="-Wl,--as-needed"
###  \#vi   /etc/portage/package.env
    ---------------CORE PACKAGES TO BUILD WITH GCC:
    sys-apps/which gcc
    sys-fs/reiserfsprogs gcc
    sys-libs/ncurses gcc
    sys-libs/zlib gcc
    sys-apps/busybox gcc
    sys-fs/e2fsprogs gcc
    sys-devel/binutils gcc
    sys-libs/glibc gcc
    sys-devel/dragonegg gcc
    dev-libs/openssl gcc
    sys-boot/grub gcc
    ---------------USER PACKAGES TO BUILD WITH GCC:
    media-libs/mesa gcc
    ---------------USER PACKAGES TO BUILD WITH clang:
    app-misc/screen clang

\###启用链接时间优化
### \#vi   /usr/local/bin/clang-ar
    #!/bin/sh
    firstarg=${1}
    shift
    exec /usr/bin/ar "${firstarg}" --plugin /usr/lib/llvm/LLVMgold.so "${@}"

### \#chmod +x   /usr/local/bin/clang-ar
### \#vi   /etc/portage/env/clang-lt

    CC='clang'
    CXX='clang++'
    CFLAGS="${CFLAGS} -O4"
    CXXFLAGS="${CXXFLAGS} -O4"
    LDFLAGS="${LDFLAGS} -O4 -Wl,-plugin,/usr/lib/llvm/LLVMgold.so"
    AR='/usr/local/bin/clang-ar'
    RANLIB=':'
    NM='nm --plugin /usr/lib64/llvm/LLVMgold.so'

\###加入distcc支持
### \#ln -s /usr/bin/distcc /usr/lib/distcc/bin/clang
### \#ln -s /usr/bin/distcc /usr/lib/distcc/bin/clang++
