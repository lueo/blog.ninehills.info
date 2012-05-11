Date: 2012-5-11
Title: Linux各种乱码的解决办法[完善中]
Slug: linux-gbk
Tags: linux, ubuntu, gbk, rar, zip
Category: Linux

Locale 设置
-----------

使用`UTF-8`编码，`en_US.UTF-8`或者`zh_CN.UTF-8`均可。可用`locale`命令查询。

	% locale
	LANG=en_US.UTF-8
	LANGUAGE=en_US:en
	LC_CTYPE=en_US.UTF-8
	LC_NUMERIC=en_US.UTF-8
	LC_TIME=en_US.UTF-8
	LC_COLLATE="en_US.UTF-8"
	LC_MONETARY=en_US.UTF-8
	LC_MESSAGES="en_US.UTF-8"
	LC_PAPER=en_US.UTF-8
	LC_NAME=en_US.UTF-8
	LC_ADDRESS=en_US.UTF-8
	LC_TELEPHONE=en_US.UTF-8
	LC_MEASUREMENT=en_US.UTF-8
	LC_IDENTIFICATION=en_US.UTF-8
	LC_ALL=

locale 设置方法各发行版各不相同，但归根结底是设置以上变量，其中`LC_ALL`有最高优先级。

查看系统中所支持的locale:

	locale -a

如果没有相应的locale：
	
	# ArchLinux反注释掉相应行
	vim /etc/locale.gen
	sudo locale-gen
	# Ubuntu下可以直接生成locale（没有locale.gen文件）
	sudo locale-gen zh_CN.GBK
	# 此时会将 zh_CN.GBK 添加到 `/var/lib/locales/supported.d/local`文件
	# 也就是说可以把这个文件当Arch上的locale.gen文件用

参考文献：

1. <https://help.ubuntu.com/community/Locale>
2. <https://wiki.archlinux.org/index.php/Locale>

rar 文件名乱码
------------

Ubuntu 12.04 用源中的`rar`包会导致在win平台创建的rar包中文文件名乱码，解决办法是使用源中的`unrar`包，或者使用 [rar for linux][rar-for-linux]。

注意这些包的原作者均为`Alexander L. Roshal`，其中`unrar`是免费软件，`rar`是共享软件，试用40天。

[rar-for-linux]: http://www.rarlab.com/download.htm

zip 文件名乱码
------------

这个主要是因为zip文件对文件名的编码默认为当前环境的locale，如在windows下压缩的zip文件，在linux下其中的中文名便会乱码。

这是zip格式的缺陷，所以目前并没有很完美的解决办法。当前的办法有如下几种：

1. 给`unzip`加`-O CP936`参数，强制制定代码页。

		% unzip -O CP936 test.zip
		//有人说unzip可能不支持-O参数，但实测UnZip 6.00可以
		% unzip -v
		UnZip 6.00 of 20 April 2009, by Debian. Original by Info-ZIP.
		//也可做alias，使解压更方便
		alias unzip-gbk='unzip -O CP936'

2. 用Python自己写一个`unzip-gbk.py`:

		#!/usr/bin/env python
		# -*- coding: utf-8 -*-
		# unzip-gbk.py

		import os
		import sys
		import zipfile

		print "Processing File " + sys.argv[1]

		file=zipfile.ZipFile(sys.argv[1],"r");
		for name in file.namelist():
		    utf8name=name.decode('gbk')
		    print "Extracting " + utf8name
		    pathname = os.path.dirname(utf8name)
		    if not os.path.exists(pathname) and pathname!= "":
		        os.makedirs(pathname)
		    data = file.read(name)
		    if not os.path.exists(utf8name):
		        fo = open(utf8name, "w")
		        fo.write(data)
		        fo.close
		file.close()