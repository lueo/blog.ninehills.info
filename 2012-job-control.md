Date: 2012-4-30
Title: Linux作业管理笔记
Slug: job-control-note
Tags: job-control, note
Category: Tools

作业管理
========

常用命令
----------

命令最后的`&`可以将程序放在后台执行，但标准输出和错误输出仍然会输出到屏幕上。

    [root@archbang ~]# tar zpcf /tmp/etc.tar.gz /etc &
    [1] 18720
    ...
    [1]+  完成                  tar zpcf /tmp/etc.tar.gz /etc

如果不想让`stdout`和`stderr`输出到屏幕上，可以用重定向来解决，如：

    # tar -zpcvf /tmp/etc.tar.gz /etc > /tmp/tar.log 2>&1 &

`ctrl-z`可以将当前作业放入后台暂停。

`jobs`则是观察当前后台作业状态。

`fg` 将后台作业拿到前台。

`bg` 将用`ctrl+z`暂停的作业在后台继续运行。

`kill` 则可以杀死进程，当然可以结束后台作业。

`nohup` 用于让命令不受`SIGHUP`信号影响，这样在退出会话/登出帐号后，
也可以继续运行命令，例如：

    nohup command > myout.file 2>&1 &

此时我们退出此shell，命令依然会在后台运行

screen 和 tmux
---------------

上述命令虽然方便，但在不稳定的SSH远程登录环境下不甚安全。
一旦SSH断开，则shell进程被杀死，其上执行的程序也均终结
（nohup命令虽然可以避免这一点，但使用不方便）。

此时我们有`screen`或者`tmux`，以screen为例：

    $ screen    # 此时便进入screen的某个会话组中
    # 用`ctrl-a-h`来获取帮助
    # 在screen会话组中，用`ctrl-a-d`，便可脱离该会话组
    # 屏幕显示[detached]
    $ screen -r # 恢复会话组
    # 掉线之类的只需重新登录后 screen -r 即可

在一个screen会话组中，允许开多个会话，这样我们就不用另行登录，
便可方便的在多个会话中执行不同的命令，并方便切换。

screen的详细使用参见 [Manual][1], [DeveloperWorks@cn][2]。

tmux和screen类似，具体使用方法参见`man tmux`。

另外也有一些以screen和tmux为后端的软件，比如`byobu`，
集成了很多方便的功能，免去了自己配置的麻烦。


[1]: http://www.gnu.org/software/screen/manual/screen.html
[2]: http://www.ibm.com/developerworks/cn/linux/l-cn-screen/

