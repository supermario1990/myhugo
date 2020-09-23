---
title: "python在invoke任务自动化工具下如何调试"
date: 2020-04-10T22:40:31+08:00
draft: false
toc: false
comments: true
categories:
- python
tags:
- python
---

<!--more-->

前段时间了解到**invoke**这个任务自动化工具，感觉很好用。所以在最近的工作中把它用了起来。感觉把很多零碎的东西用invoke的task整合起来，效率是提高很多。这里就不介绍invoke的使用方法，详细请参考[官网文档](http://www.pyinvoke.org/)

我执行task时，发现task中的流程并没有执行完任务就结束了。只执行到这句代码就结束了：

```python
rs = c.run('service postgresql status')
```

很奇怪，这么简单的一句话，检查下postgresql数据库的状态，能出什么问题？invoke在执行的时候也没有打印出堆栈。

所以我就想调试下程序，看下到底是哪里出了问题？但是怎么调试呢？通常我们在写python程序时，用vscode或者pycharm就能很轻松的进行调试。使用命令python xxx.py 就能运行。但是invoke下使用的命令是：

```python
inv commond
```

网上查了下，有网友和我有一样的疑惑，还在github上提了issue问作者怎么调试。但是被作者一顿怼，说这并不是代码的问题，请不要在这里问。网上也查不到其他的答案。然后我就用看了下帮助文档：

```python
inv -h
Usage: inv[oke] [--core-opts] task1 [--task1-opts] ... taskN [--taskN-opts]

Core options:
  -d, --debug                        Enable debug output.
```

果然有个-d的参数可以用：

```python
inv -d commond
```

这样一运行，打印了很多信息。最后我看到了问题，原来是c.run('service postgresql status') 抛出了一个异常，而c.run()内部把异常catch掉了，所以就没有打印任何信息。

所以加-d参数，是调试的一个方法。

下面要介绍另一种方法：pdb。pdb看名字就像是gdb，用法也和gdb类似

简单说下pdb的用法，比如我有一段代码如下：

```python
import pdb

a = 1
pdb.set_trace()
b = 2
print('a + b =', a + b)
```

1. 首先impot pdb库
2. 在 pdb.set_trace() 表示在*a = 1* 这代码后加了一个端点

当代码于运行到pdb.set_trace()时会自动停住：

```python
D:\mypython\mypdb>python pdbtest.py
> d:\mypython\mypdb\pdbtest.py(5)<module>()
-> b = 2
(Pdb) p a
1
(Pdb) n
> d:\mypython\mypdb\pdbtest.py(6)<module>()
-> print('a + b =', a + b)
(Pdb) p b
2
(Pdb)
```

p a：可以查看a变量的值

n：表示下一步

...

具体pdb的用法，请参见使用文档，本文仅仅提供一种调试方法。