# 使用hugo

## hugo使用
> 不经常用hugo，导致每次使用都要重新翻找教程，干脆把要用的操作写下来备份

1. 创建新文章：

```text
hugo new posts/my-first-post.md
```

这里面值得注意的是，通过上述命令行创建的文章中，会自动生成一部分文本如下：

```text
---
title: "My First Post"
date: 2019-03-26T08:47:11+01:00
draft: true
---
```

**我们需要把 draft : true 修改成 draft : false 才可以上传这篇文章(draft为草稿)**

还可以添加描述，分类，标签等，使用如下

```text
---
description: "一篇关于xxx文章"
tags: [
    "markdown",
    "test",
] 
categories: [
    "theme",
    "未分类"
]
---
```


2. 打包网站到 /docs 文件夹

```text
hugo -d docs
```



3. 上传代码至 master

```text
git commit -am "updates $(date)"
git push origin master
```
遇到的问题
使用git push命令提交更改报错 fatal: unable to access 'http://github.com/xxxxxx/': OpenSSL SSL_read: xxx
443错误或者10054错误

最后使用ssh上传解决了。

解决方法参考：[使用git push命令提交更改报错 fatal: unable to access ‘http://github.com/xxxxxx/‘: OpenSSL SSL_read: Connection](https://blog.csdn.net/haoah316/article/details/118873748?utm_medium=distribute.pc_relevant.none-task-blog-2~default~baidujs_title~default-0.no_search_link&spm=1001.2101.3001.4242.1)


ssh key的配置参考：[GitHub如何配置SSH Key](https://blog.csdn.net/u013778905/article/details/83501204)


### 参考
本文参考：[使用 Hugo + Github 搭建个人博客](https://zhuanlan.zhihu.com/p/105021100)


