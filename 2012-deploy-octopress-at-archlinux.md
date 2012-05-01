Date: 2012-4-17
Title: Octopress在ArchLinux上的安装笔记
Slug: deploy-octopress
Tags: jekyll, github, ruby, markdown, octopress, goagent
Category: Tools

## 使用GoAgent加速安装 ##

国内尤其是教育网出国的速度很慢，而且还要时不时受墙的干扰。感谢Google和GoAgent，给了这些迷途羔羊以光明。

安装GoAgent[3]的过程此处不提，只谈谈如何在命令行设置http代理，此时curl/wget/rvm/gem/bundle均会自动适用。
    
    # 设定http和https代理，指向GoAgent
    export http_proxy='http://127.0.0.1:8087'
    export https_proxy=$http_proxy
    # 让curl永久忽略SSL错误
    echo insecure >> ~/.curlrc
    # 让git clone时忽略SSL错误（必须clone的是https源）
    export GIT_SSL_NO_VERIFY=true
    # 全局设定
    git config --global http.sslVerify false
    # OK，其他的不用设定了，比如rvm 会调用curl来下载。

## 安装Ruby环境 ##

    #安装 git
    sudo pacman -S git
    #安装 rvm[2]
    curl https://raw.github.com/wayneeseguin/rvm/master/binscripts/rvm-installer | bash -s stable
    # 在 .bashrc/.zshrc 加入RVM函数
    echo '[[ -s $HOME/.rvm/scripts/rvm ]] && source $HOME/.rvm/scripts/rvm' >> ~/.zshrc
    source ~/.zshrc
    # 安装依赖包（下面命令会输出一个指南，直接复制其中的命令就行）:
    rvm requirements
    # 比如ArchLinux上一步会输出：
    sudo pacman -Sy --noconfirm gcc patch curl zlib readline libxml2 libxslt git autoconf automake diffutils make libtool bison subversion
    # 安装ruby 1.9.2 并使用
    rvm install 1.9.2 && rvm use 1.9.2
    # 保持rubygems最新
    rvm rubygems latest
    # 检查ruby版本是否为1.9.2
    ruby -v

## 安装Octopress ##

    # clone octopress
    git clone https://github.com/imathis/octopress.git octopress
    cd octopress
    # 安装依赖
    gem install bundler
    bundle install
    # 安装默认主题
    rake install
    # 设定octopress，参见[4]
    # ......
    # 生成博客
    rake generate #生成
    rake watch    #实时观察变更
    rake preview  #在http://localhost:4000上预览
    # 配置github
    rake setup_github_pages #按提示操作即可
    # 推送博客
    rake deploy
    # 或者
    rake push

## Bugs ##

1. `rake generate`时报错

        rake aborted!
        You have already activated rake 0.9.2.2, but your Gemfile requires rake 0.9.2. Using bundle exec may solve this.

    这是因为`Gemfile.lock`不更新的原因，用`bundle update`可更新，但octopress不推荐更新`Gemfile.lock`。解决办法是用以下命令来替代：
        
        # 其他rake命令也要如此，直到上游更新了Gemfile.lock。
        bundle exec rake generate

2. 其余可能出现的bug：
    1. <http://blog.gonzih.org/blog/2011/09/21/fix-octopress-pygments-error-on-arch-linux/>
    2. <http://jgarber.lighthouseapp.com/projects/13054/tickets/245-error-installing-redcloth-428>

## Reference ##

1. <http://octopress.org/docs>
2. <http://beginrescueend.com/rvm/install/>
3. <https://code.google.com/p/goagent/>
4. <http://octopress.org/docs/configuring/>