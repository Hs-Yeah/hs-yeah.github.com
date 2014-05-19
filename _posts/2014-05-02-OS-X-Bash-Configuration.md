---
layout: post
title: "OS X自带Bash装修工程"
categories : [OS X]
tags: [OS X, Bash, .bash_profile]
---

晚上闲得无（dàn）聊（téng），折腾了一下 OS X 终端所用的Bash的配置   
  
结果如下：  
![](/assets/images/posts/OS X Bash Configuration - Screenshot.png)  
所用的主题是 [Pro Tango](https://www.dropbox.com/s/tirgxm3id9ykq2i/Pro%20Tango.terminal)，来自 [V2EX](http://www.v2ex.com/t/91336) 
  
而终端提示符的修改则是在 `~/.bsh_profile` 里面加入下面这行语句：

	export PS1='\n\[\e[0;30m\]┌─\[\e[0m\]\[\e[01;30m\]\u@\h\[\e[00m\]:\[\e[01;34m\]\w\[\e[00m\]\[\e[01;30m\] `if [ $? = 0 ]; then echo "\[\e[32m\]✔"; else echo "\[\e[31m\]✘"; fi`\n\[\e[0;30m\]└───\[\e[0m\]\$ \[\e[00;33m\]`git branch 2> /dev/null | grep -e ^* | sed -E  s/^\\\\\*\ \(.+\)$/\(\\\\\1\)\/`\[\e[00m\] '
	
上面这条语句里面调用了 git，所以需要注意：  
如果使用过 Xcode，则 git 已安装；  
如果没有安装 Xcode，请从 Mac App Store 下载 Xcode 并在偏好设置安装 Command Line Tools