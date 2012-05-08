Date: 2012-5-8
Title: Sublime Text 2 in Linux
Slug: sublime-text-in-linux
Tags: linux, ubuntu, sumlime-text
Category: Linux

[Sublime Text 2][sublime-text-2]是一款很不错的代码编辑器，而且支持全平台。

安装
----
Linux下Sublime Text 2可以直接在官网下载，建议下载[Dev版][dev]或[nightly版][nightly]。

ArchLinux和Ubuntu也可以利用第三方源来安装，但均不包含二进制包，而是需要在`makepkg`/`dpkg configure`时从官网下载二进制包。

    # ArchLinux
    yaourt -S aur/sublime-text
    yaourt -S aur/sublime-text-dev
    yaourt -S aur/sublime-text-nightly
    # Ubuntu
    # see: http://www.webupd8.org/2011/03/sublime-text-2-ubuntu-ppa.html
    sudo add-apt-repository ppa:webupd8team/sublime-text-2
    sudo apt-get update
    sudo apt-get install sublime-text-2-dev

配置
----

这篇文章基本搜罗全：[编辑器大作战——Sublime Text 2][1]。

我的配置：

1. 安装[Package Control][2]插件
2. 用`Ctrl+Shift+P`调出命令模式，输入install回车后便可安装插件
3. 推荐插件：
    * git
    * markdown preview
    * gist

F.A.Q
-----

1. 中文输入法支持！
    fcitx 4.2.0 (Ubuntu 12.04 官方源)+ sublime text 2 Build 2195。可以中文输入。
    问题是退格键不是删fcitx面板里的拼音，而是直接删sublime中的字，同时回车键也被sublime给抢占了。


[sublime-text-2]: http://www.sublimetext.com/
[dev]: http://www.sublimetext.com/dev
[nightly]: http://www.sublimetext.com/nightly
[1]: http://ruby-china.org/topics/3107
[2]: http://wbond.net/sublime_packages/package_control/installation
