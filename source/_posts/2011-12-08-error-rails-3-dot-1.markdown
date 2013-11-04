---
layout: post
title: "伤不起的Rails 3.1"
date: 2011-12-08 17:14
comments: true
categories: Ruby Rails
--- 
Maybe很多ror新人如我会跟着那个[Rails guide](http://guides.RubyonRails.org/)小试牛刀搭一个自己的blog.一步步下来小有体会到Rails的强劲,也没出啥问题,到的comment那一块,X,对了半天源代码甚至复制粘贴到Sublime Text,奈何还是坑爹的冒出错误:
```
undefined method `error_messages' for #<ActionView::Helpers::FormBuilder:0x3bff910>
```

嗯,好吧,简体和繁体的guide都是基于Rails 3.0的.于是去官方看了下Rails 3.1的那个guide.很明显的,源代码少了  
`"<%= f.error_messages %>"`  
这一行.删去后妥妥的....  
查了下`error_messages`这玩意的用处,没查到,有木有高端指教一下啊~~  
好吧,顺便借机叹息一下,中文的ror资料更新的好慢啊,想看中文版的AWDWR第四版却迟迟不出.难不成真的要去看英文版的??最近看 github上的英文已经各种够吃力了!!~~~~(>_<)~~~~ 