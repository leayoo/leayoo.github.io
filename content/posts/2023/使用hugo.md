---
title: "使用hugo"
date: 2021-12-21T11:47:47+08:00
draft: false
description: "一篇关于hugo使用的文章"
categories: [
    "hugo"
]
tags: [
    "markdown",
    "hugo"
] 
featuredImagePreview: https://w.wallhaven.cc/full/9m/wallhaven-9mjoy1.png
summary: "hugo使用 不经常用hugo，导致每次使用都要重新翻找教程，干脆把要常用的操作写下来备份"
---
![pic](https://w.wallhaven.cc/full/9m/wallhaven-9mjoy1.png)
## hugo使用
> 不经常用hugo，导致每次使用都要重新翻找教程，干脆把要常用的操作写下来备份

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

更多前置参数

- **title**: 文章标题.
- **subtitle**: 文章副标题.
- **date**: 这篇文章创建的日期时间. 它通常是从文章的前置参数中的 `date` 字段获取的, 但是也可以在 [网站配置](https://hugoloveit.com/zh-cn/theme-documentation-basics#site-configuration) 中设置.
- **lastmod**: 上次修改内容的日期时间.
- **draft**: 如果设为 `true`, 除非 `hugo` 命令使用了 `--buildDrafts`/`-D` 参数, 这篇文章不会被渲染.
- **author**: 文章作者.
- **authorLink**: 文章作者的链接.
- **description**: 文章内容的描述.
- **license**: 这篇文章特殊的许可.
- **tags**: 文章的标签.
- **categories**: 文章所属的类别.
- **featuredImagePreview**: 用在主页预览的文章特色图片.
- **summary**: 文章摘要

1.5 如果文章 untrack 
```text
git add .
```

2. 打包网站到 /docs 文件夹

```text
hugo -d docs
```



3. 上传代码至 master

```text
git commit -am "updates $(date)"
git push origin master 或者 git push
```
遇到的问题
使用git push命令提交更改报错 fatal: unable to access 'http://github.com/xxxxxx/': OpenSSL SSL_read: xxx
443错误或者10054错误

最后使用ssh上传解决了。

解决方法参考：[使用git push命令提交更改报错 fatal: unable to access ‘http://github.com/xxxxxx/‘: OpenSSL SSL_read: Connection](https://blog.csdn.net/haoah316/article/details/118873748?utm_medium=distribute.pc_relevant.none-task-blog-2~default~baidujs_title~default-0.no_search_link&spm=1001.2101.3001.4242.1)


ssh key的配置参考：[GitHub如何配置SSH Key](https://blog.csdn.net/u013778905/article/details/83501204)


### 参考
本文参考：[使用 Hugo + Github 搭建个人博客](https://zhuanlan.zhihu.com/p/105021100)
[主题文档-内容](https://hugoloveit.com/zh-cn/theme-documentation-content/)
