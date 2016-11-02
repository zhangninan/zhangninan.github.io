---
layout: post
title: "Octopress + github搭建个人博客"
date: 2016-11-01 17:21:44 -0400
comments: true
categories: 
---

###安装ruby出现的问题
具体搭建方法我是参考[Jony Fang](jonyfang.github.io)的方法，本文主要记录配置Octopress的环境所遇到的问题。

当我执行
```
rvm rubygems latest
```
命令时
我的Mac自带ruby 2.0.0, 但是rvm没办法找到系统中安装的ruby，会报如下错误：

	Error running 'env GEM_HOME=/Users/zhangninan/.rvm/gems/system@global
	GEM_PATH= ~/.rvm/rubies/system/bin/ruby -d ~/.rvm/src/rubygems-2.4.8/setup.rb --no-document',
	showing last 15 lines of ~/.rvm/log/1477946222_system/rubygems.install.log
	[2016-10-31 16:37:02] ~/.rvm/rubies/system/bin/ruby
	current path: ~/.rvm/src/rubygems-2.4.8
	PATH=/usr/local/bin:/usr/bin:/bin:/usr/sbin:/sbin:/usr/texbin:/Users/zhangninan/.rvm/bin
	command(7): env GEM_HOME=~/.rvm/gems/system@global GEM_PATH= ~/.rvm/rubies/system/bin/ruby -d ~/.rvm/src/rubygems-2.4.8/setup.rb --no-document
	env: ~/.rvm/rubies/system/bin/ruby: No such file or directory

解决办法是在rvm里面再安装一个ruby  
command： ``rvm install ruby --head``  
但是，又出现了``\bin``无法写入的问题： 

	ERROR: '/bin' is not writable - it is required for Homebrew, try 'brew doctor' to fix it!
	Requirements installation failed with status: 1.
  
尝试了``brew doctor``，报错：

	/System/Library/Frameworks/Ruby.framework/Versions/2.0/usr/lib/
	ruby/2.0.0/rubygems/core_ext/kernel_require.rb:55:in `require': 
	cannot load such file -- mach (LoadError)

homebrew的问题，我尝试了  
``$ sudo chown -R $(whoami):admin /usr/local``

但是还是没有办法安装。我又从[stackoverflow](http://stackoverflow.com/questions/24652996/homebrew-not-working-on-osx)上找到了解决办法。

~~~
1. open terminal  
2. $ cd /usr/local  
3. $ git reset --hard  
4. $ git clean -df
5. $ brew update
~~~
至此，再执行 ``rvm install ruby --head`` 就没有问题了。  
