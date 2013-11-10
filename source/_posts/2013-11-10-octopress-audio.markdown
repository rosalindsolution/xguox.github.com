---
layout: post
title: "Octopress 添加 Audio"
date: 2013-11-10 16:07
comments: true
categories: Octopress
---

又是忽然的,想贴音乐在 Octopress ,但是发现貌似自带的插件只有 video_tag 贴视频. 搜索了一下发现一个合适的. 直接贴 Gist 了(尴尬,才发现 Gist 的样式貌似跟主题冲突了,又一个 TODO ):

{% gist 3487038 audio_tag.rb %}

语法跟原本的 `video_tag` 差不多, 


```
        {% raw %}{% audio url/to/mp3 %}{% endraw %}
        {% raw %}{% audio url/to/mp3  url/to/ogg %}{% endraw %}
```

{% audio http://m1.file.xiami.com/259/58259/71695147/1771959699_10559682_l.mp3 %}

有了插件支持,还得有 mp3 的 url, 听歌真心不多, 偶尔用的多的是虾米, 但却没处找上边的音乐的真实地址, 后得大神提醒, 在浏览器的 Inspector 可以查找到, 以 Chrome 为例, 在 Dev Tool 的 'Network' 可以轻易找到相应音乐的链接.  
还有更流弊的做法参考 [http://www.cnblogs.com/bobzhou/archive/2013/02/16/bobzhou.html](http://www.cnblogs.com/bobzhou/archive/2013/02/16/bobzhou.html)
