---
title: "Python Scrapy爬虫分页"
date: 2019-06-19T17:34:31+08:00
draft: false
toc: false
comments: true
categories:
- python
- 爬虫
tags:
- python
- 爬虫
---

<!--more-->

最近学习了下python爬虫框架**scrapy**，初学踩了些坑。由于不是爬虫教程，所以免去一些上下文的交代，后面有若有时间再写一写爬虫方面的心得。这里仅仅简单的记录下自己遇到的问题

1. 对分页的处理

分页处理关键在于**parse**函数怎么写，定义如下：

```python
def parse(self, response)
```

先来看看我之前是怎么写的：

```python
def parse(self, response):
    # 这里对items进行处理 
    self.parse_item(response) # 1.这一步有不对，一定要写成 yeild self.parse_item(response) 
    # 获取下一页的url
    next_page = self.get_next_pageurl() # 2
    if next_page: # 3
        yield Request(next_page, callback=self.parse) # 4
```

对，错误就在于1代码，一定要加上关键字 yield。没有yield，则scrapy框架没法对parse函数结果进行遍历处理。

