Date: 2012-4-16
Title: Jekyll相关资源链接
Slug: jekyll-links
Tags: jekyll, github, ruby, markdown
Category: Tools

>同时发布在V2EX: <http://www.v2ex.com/t/32351>

Jekyll
------
**主页：** <http://jekyllrb.com/>  
**项目地址**：<https://github.com/mojombo/jekyll>  
GitHub Pages 原生支持: <http://help.github.com/pages/>  
迁移向导：<https://github.com/mojombo/jekyll/wiki/blog-migrations> 

衍生
----
Jekyll-Bootstrap: <http://jekyllbootstrap.com/>  
（相当于jekyll的封装，比octopress更接近原生）    
Octopress: <http://octopress.org/>  
（部署简单方便，推荐。比较大的问题是会把ruby部分和你的博客混在同一个git源里） 

类似应用
--------
pelican（python实现）: <http://pelican.readthedocs.org/en/2.8/index.html>  
基于Pelican和Dropbox的Blog服务：<http://calepin.co/> （免费CNAME，但是国内速度慢） 

模版/实现
---------
Tom: <https://github.com/mojombo/mojombo.github.com>  
简洁风格: <https://github.com/geeklu/geeklu.github.com>  
Jekyll-Bootstrap的主题: <http://themes.jekyllbootstrap.com>  

其余
----
Ruby包管理使用GoAgent加速：<http://ninehills.info/2011/12/17/ruby-use-proxy.html>  
Ruby环境部署：RVM+Bundler（参见octopress的[部署向导][1] ArchLinux的[Bug1][2] [Bug2][3]） 

**P.S.** 注意octopress最好不要使用`bundle update`，即需要保持`Gemfile.lock`不变，这样出问题的概率最小。

[1]: http://octopress.org/docs/setup/ 
[2]: http://blog.gonzih.org/blog/2011/09/21/fix-octopress-pygments-error-on-arch-linux/ 
[3]: http://jgarber.lighthouseapp.com/projects/13054/tickets/245-error-installing-redcloth-428 