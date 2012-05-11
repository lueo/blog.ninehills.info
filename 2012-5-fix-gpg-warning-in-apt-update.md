Date: 2012-5-11
Title: 解决apt-get update 时的 GPG Warning
Slug: fix-gpg-warning-in-apt-update
Tags: linux, ubuntu, gbk, rar, zip
Category: Linux

Ubuntu 12.04 `apt-get update`时出现`GPG Warning`:

	W: GPG error: http://ppa.launchpad.net precise Release: The following signatures were invalid: BADSIG 6AF0E1940624A220 Launchpad PPA for TualatriX

因为添加第三方源一直是用`sudo add-apt-repository`来添加的，所以按理说不应该出现这个问题。

解决办法：(清空本地软件包列表，不知道为什么可以)

	sudo rm -rf /var/lib/apt/lists
	sudo apt-get clean
	sudo apt-get update

如果还是出现问题，可以考虑手动导入Key：

	sudo apt-key adv --keyserver keyserver.ubuntu.com --recv-keys 6AF0E1940624A220

see: <http://ubuntuforums.org/showthread.php?t=1661773>