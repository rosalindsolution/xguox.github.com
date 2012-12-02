---
layout: post
title: "Ruby/Rails.note"
date: 2012-02-27 17:55
comments: true
categories: Script
---
把平常中一些开发出错以及解决方法记录了下来，其实，基本上都是Google或者StackOverflow得到的答案。然后有些都不知道问题的根源，只知道个解决方法

*
{% codeblock %}Issue--
	RVM is not a function, selecting rubies with 'rvm use ...' will not work.
{% endcodeblock %}
{% codeblock %}Solution--
	添加下面这句到  ~/.bashrc
	[[ -s "$HOME/.rvm/scripts/rvm" ]] && . "$HOME/.rvm/scripts/rvm"重启终端
{% endcodeblock %}

*
<!-- more -->
{% codeblock %}Issue--
	Gem::Installer::ExtensionBuildError: ERROR: Failed to build gem native extension.
	.
	.
	An error occured while installing mysql2 (0.3.11), and Bundler cannot continue.

	Make sure that `gem install mysql2 -v '0.3.11'` succeeds before bundling.
{% endcodeblock %}
{% codeblock %}Solution--
sudo apt-get install libmysql-ruby libmysqlclient-dev （Ubuntu）
{% endcodeblock %}

*
{% codeblock %}Issue--
Could not find a JavaScript runtime. See https://github.com/sstephenson/execjs for a list of available runtimes.
{% endcodeblock %}
{% codeblock %}Solution--
Just install execjs and the rubyracer in your gemfile and run bundle after.
gem 'execjs'
gem 'therubyracer'
{% endcodeblock %}

*
{% codeblock %}Issue--
>Can't connect to local MySQL server through socket '/tmp/mysql.sock' (2)
Couldn't create database for {"adapter"=>"mysql2", "encoding"=>"utf8", "database"=>"o_p_", "pool"=>5, "username"=>"root", "password"=>nil, "socket"=>"/tmp/mysql.sock"}, charset: , collation: 
{% endcodeblock %}
{% codeblock %}
Solution--
host：127.0.0.1（diff？？localhost）
{% endcodeblock %}

*
{% codeblock %}Issue--
Gem::Installer::ExtensionBuildError: ERROR: Failed to build gem native extension.

/usr/bin/ruby1.9.1 extconf.rb 
checking for pg_config... no
No pg_config... trying anyway. If building fails, please try again with
--with-pg-config=/path/to/pg_config
checking for libpq-fe.h... no
Can't find the 'libpq-fe.h header
* extconf.rb failed *
Could not create Makefile due to some reason, probably lack of
necessary libraries and/or headers. Check the mkmf.log file for more
details. You may need configuration options.


sudo apt-get install libpq-dev

sudo gem install pg
{% endcodeblock %}
{% codeblock %}
Solution--
sudo apt-get install libpq-dev
sudo gem install pg

#貌似跟之前那个mysql的问题有点像
{% endcodeblock %}

{% codeblock %}
除了系统的ruby文件可以用相对路径，自己编写的ruby文件如果要require的话需要用绝对路径。否则会报错no such file to load .
而load则无论什么ruby文件都可以用相对路径加载
{% endcodeblock %}