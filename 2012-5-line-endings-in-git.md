Date: 2012-5-10
Title: Line endings in git
Slug: line-endings-in-git
Tags: git
Category: Git
Status: draft

跨平台使用git最大的问题之一便是`Line Endings`。传统的[解决办法][1]是设置`autocrlf`：

	# Linux & Mac OSX
	git config --global core.autocrlf input
	# Windows
	git config --global core.autocrlf true

这样可以保证git源中的行尾符统一为`LF`。但是很容易将`CRLF`的文件混入源中，搞的比较麻烦。

Git 1.7.2+ 版本引入了`.gitattributes`文件，保证不会把`CRLF`文件推送至源中。

增加`.gitattributes`文件，内容为：

	* text=auto

然后将其add后commit即可。

F.A.Q
-----

1. 如何将现有的源的行尾符统一为`LF`?

	参见[`man gitattributes`][gitattributes]：

		$ echo "* text=auto" >>.gitattributes
		$ rm .git/index     # Remove the index to force git to
		$ git reset         # re-scan the working directory
		$ git status        # Show files that will be normalized
		$ git add -u
		$ git add .gitattributes
		$ git commit -m "Introduce end-of-line normalization"

2. git对文件的类型探测错误，比如将某个pdf文件识别为text.

	修改`gitattributes`文件：

		# 将*.jpg设置为非text类型
		*.jpg           -text
		# 将*.txt设置为text类型
		*.txt 			text
		# 强制指定某种文件的EOL
		*.vcproj        eol=crlf

[gitattributes]: http://man.github.com/git/gitattributes.html
[1]: http://help.github.com/line-endings/