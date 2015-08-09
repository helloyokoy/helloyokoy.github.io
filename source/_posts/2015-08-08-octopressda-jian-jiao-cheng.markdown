---
layout: post
title: "octopress搭建教程"
date: 2015-08-08 21:00:18 +0800
comments: true
categories: octopress
---


1.安装Octopress
---
	git clone git://github.com/imathis/octopress.git octopress
	cd octopress
	bundle update    # 安装依赖的组件
	rake install     # 安装默认的Octopress主题

2.配置Git
---
值得注意的是这里git的origin已经存在，并且指向octopress的master分支的，这里为了方便进行了更改：

	git remote rm origin
	git remote add origin git@github.io:helloyokoy/stormzhang.github.com.git
	git remote add octopress git://github.com/imathis/octopress.git  # 为了octopress的升级而添加

<!--more-->

3.配置github
---
在github上创建一个仓库，注意仓库名称要以下这种格式yourname.github.io，这样代码发布后自动这个url就可以访问了（此处一定要注意哦，我刚开始没注意，死活没得到想要的效果）。 

例如你的 GitHub 帐号是 jack 就将 Repository 命名为 jack.github.io， 完成后会得到一组 GitHub Pages URL http://yourname.github.io/ (注意不能用 https协议，必须用 http协议)。

设定 GitHub Pages

	rake setup_github_pages
	以上执行后会要求 read/write url for repository ：

git@github.com:yourname/yourname.github.io.git

	rake generate
	rake deploy


4.将 source 也加入 git
---
	git add .
	git commit -m 'initial source commit'
	git push origin source
	
5.更新 Octopress
---
	git remote add octopress git://github.com/imathis/	octopress.git
	git pull octopress master     # Get the latest Octopress
	bundle install                # Keep gems updated
	rake update_source            # update the template's 	source
	rake update_style             # update the template's style
	
6.发表新文章
---
	rake new_post["新文章名称"]
	rake preview
	用浏览器打开 http://localhost:4000 就可以看到效果了。

7.发布
---

	rake gen_deploy
	rake deploy                 #若发布后无效果可试试此命令
