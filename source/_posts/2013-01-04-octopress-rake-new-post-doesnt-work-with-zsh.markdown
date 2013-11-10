---
layout: post
title: "Octopress' Rake New_post Doesn't Work With Zsh"
date: 2013-01-04 20:06
comments: true
categories: Octopress
---
原文---[传送门]( http://ryanarneson.com/blog/2012/04/07/rake-new-post-doesnt-work-with-zsh/)

 近日,在医院闲来无事写点东西,才发现,在**zsh(oh-my-zsh)**下,原本用于生成一篇Octopress Post的命令```rake new_post["My new post"]```不管用鸟,各种报错
```
zsh: no matches found: new_post[My new post]
```
<!-- more -->
如果你对个中缘由感兴趣的话,这有个[传送门](https://github.com/imathis/octopress/issues/117#issuecomment-3707975)

如果你木有兴趣知道,那想要解决也很简单,把命令换成```rake new_post\["My new post"\]```

由传送门对面的讨论可知, [imathis](https://github.com/imathis)准备把**new_post**命令更新成以下这样:

```
rake new_post
Enter a post title:
```