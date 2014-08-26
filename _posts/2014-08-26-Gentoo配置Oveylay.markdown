---
title: Gentoo配置Oveylay 
layout: post
author:
  name: macint0sh
---
1.安装 layman

    # emerge layman
    # echo "source /var/lib/layman/make.conf" >> /etc/portage/make.conf
    
2.配置 overlay
    
    # mkdir -p /var/lib/layman/local/{metadata,profiles}
    # echo 'local' > /var/lib/layman/local/profiles/repo_name
    # echo 'masters = gentoo' > /var/lib/layman/local/metadata/layout.conf

编辑 /var/lib/layman/make.conf加入/var/lib/layman/local使它看起来像这样：    

    PORTDIR_OVERLAY="
    /var/lib/layman/local
    $PORTDIR_OVERLAY
    "
3.将ebuild加入overlay

    # mkdir -p /var/lib/layman/local/games-util/steam
    # cp steam-1.0.0.47.ebuild /var/lib/layman/local/games-util/steam/steam-1.0.0.47.ebuild
    # pushd /var/lib/layman/local/games-util/steam
    # repoman manifest
    # popd



