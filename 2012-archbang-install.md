Date: 2012-4-20
Title: ArchBang 2011.11 安装笔记
Slug: archbang-install
Tags: linux, archlinux, guide, installation, note
Category: Linux

[ArchBang][1]是一个不错的[Archlinux][2]定制版本，但它最新的iso为2011.11版。而Archlinux在此后有一些需要手动升级的部分，见[News][3]，如果不按照News上的步骤来，很有可能就会出现各种奇妙的问题。

本文就简单把ArchBang的相关配置流程过一遍，大家只需要`Ctrl+C`和`Ctrl+V`即可。

安装更新
========

ArchBang安装完成进入桌面后，第一步先删除掉显卡驱动

    #移除所有显卡驱动（直到下一节安装驱动，不要重启。。。当然重启倒也没什么）
    sudo pacman -R xf86-video-apm xf86-video-ark xf86-video-ast\
    xf86-video-ati xf86-video-chips xf86-video-cirrus\
    xf86-video-dummy xf86-video-fbdev xf86-video-glint\
    xf86-video-i128 xf86-video-i740 xf86-video-intel\
    xf86-video-mach64 xf86-video-mga xf86-video-neomagic\
    xf86-video-nv xf86-video-r128 xf86-video-rendition\
    xf86-video-s3 xf86-video-s3virge xf86-video-savage\
    xf86-video-siliconmotion xf86-video-sis xf86-video-sisusb\
    xf86-video-tdfx xf86-video-trident xf86-video-tseng\
    xf86-video-v4l xf86-video-vesa xf86-video-vmware\
    xf86-video-voodoo xf86-video-xgi xf86-video-xgixp\
    ati-dri intel-dri mach64-dri mga-dri r128-dri\
    savage-dri sis-dri tdfx-dri

然后更新系统，步骤如下：

    pacman -Syy
    pacman -S pacman
    mv /etc/pacman.conf.pacnew /etc/pacman.conf
    pacman-key --init
    pacman -S filesystem --force
    #因为cairo系列主干已经merge了Ubuntu的补丁，故去除掉*-ubuntu。
    #如果不想移除的话，务必要重新编译安装cairo-ubuntu
    pacman -S --asdeps freetype2 libxft cairo fontconfig
    pacman -S librsvg
    pacman -Syu

配置系统
========

1. 配置显卡驱动

        rm /etc/X11/xorg.conf.d/20-gpudriver.conf
        #找到显卡型号
        lspci | grep VGA
        #搜索软件包
        pacman -Ss xf86-video | less
        #安装相应的驱动
        pacman -S your_video_driver
        #删掉内核参数'nomodeset'（开启KMS）
        nano /boot/grub/menu.lst 

2. 安装yaourt

        wget https://aur.archlinux.org/packages/pa/package-query/package-query.tar.gz
        wget https://aur.archlinux.org/packages/ya/yaourt/yaourt.tar.gz
        tar zxvf xx.tar.gz
        makepkg -s
        sudo pacman -U xx.xz
        #[可选]加速makepkg
        sudo pacman -S axel
        #edit /etc/makepkg.conf
        DLAGENTS=('ftp::/usr/bin/axel -n 20 -a -o %o %u'
                  'http::/usr/bin/axel -a -n 20 -o %o %u'
                  'https::/usr/bin/axel -a -n 20 -o %o %u'
                  ...

3. 安装VIM
    
        sudo pacman -S gvim
        #中文文档
        yaourt -S aur/vimcdoc-svn
        #基本vimrc
        set nocompatible
        syntax enable
        set encoding=utf-8
        set showcmd                     " display incomplete commands
        filetype plugin indent on       " load file type plugins + indentation

        "" Whitespace
        set nowrap                      " don't wrap lines
        set tabstop=2 shiftwidth=2      " a tab is two spaces
        set expandtab                   " use spaces, not tabs
        set backspace=indent,eol,start  " backspace through everything in insert mode

        "" Searching
        set hlsearch                    " highlight matches
        set incsearch                   " incremental searching
        set ignorecase                  " searches are case insensitive...
        set smartcase                   " ... unless they contain at least one capital letter

4. 中文配置
    
        #[可选]add locale to `.xinitrc`(for slim)
        export LANG=zh_CN.UTF-8
        export LC_ALL="zh_CN.UTF-8"
        #view font list
        fc-list | sed 's,:.*,,' | sort -u
        #install wqy-microhei
        yaourt -S aur/wqy-microhei

5. fcitx输入法

        sudo pacman -S fcitx
        #add this to `.profile`(通用)/`.xinitrc`(slim/startx)/`.xprofile`(gdm/kdm)
        #setup XIM environment, needn't if use SCIM as    gtk-immodules
        export XMODIFIERS="@im=fcitx"
        export GTK_IM_MODULE=xim
        export QT_IM_MODULE=xim
        #add this to `.config/openbox/autostart`
        killall fcitx
        fcitx &
        #英文locale下fcitx会在gtk2程序内无法激活，解决办法见下一节

Bugs
====

1. 英文locale下fcitx会在gtk2程序内无法激活

        #http://fcitx.github.com/handbook/faq.html#ctrl_space
        gtk-query-immodules-2.0 > /etc/gtk-2.0/gtk.immodules
        #edit gtk.immodules
        "xim" "X Input Method" "gtk20" "/usr/share/locale" "en:ko:ja:th:zh"

2. (Maybe)IPV6 not work.

        vim /etc/modprobe.d/modprobe.conf
        #comment next line
        blacklist ipv6

[1]: http://archbang.org/
[2]: http://www.archlinux.org/
[3]: http://www.archlinux.org/news/
