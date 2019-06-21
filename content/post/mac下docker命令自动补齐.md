---
title: "Mac下docker命令自动补齐"
date: 2019-01-02T23:29:51+08:00
draft: false
toc: false
comments: true
categories:
- 工具使用
tags:
- mac
- docker
---

<!--more-->

在mac下使用docker时发现使用tab无法补全命令，而之前在ubuntu下不会有这种情况。网上很多答案都似是而非。最后还是去官网找到了正确答案。

我使用的是Zsh，在Zsh中，是[completion system](http://zsh.sourceforge.net/Doc/Release/Completion-System.html) 在管理命令补全. 所以要实现补全docker命令，下面的文件需要复制并连接到你的Zsh `site-functions/` 文件夹中。如果你是用Homebrew安装的Zsh的话，则使用下面命令：

```shell
etc=/Applications/Docker.app/Contents/Resources/etc
ln -s $etc/docker.zsh-completion /usr/local/share/zsh/site-functions/_docker
ln -s $etc/docker-machine.zsh-completion /usr/local/share/zsh/site-functions/_docker-machine
ln -s $etc/docker-compose.zsh-completion /usr/local/share/zsh/site-functions/_docker-compose
```

执行了命令后，`source ~/.zshrc`就可以了。

**总结**：

 1. 有问题可以尝试利用搜索引擎直接找解决方案

 2. 如果试了几次问题都不能很好的解决问题，请直接查看官方文档

 3. 有条件少用百度，尽量使用英文搜索

    

​	
