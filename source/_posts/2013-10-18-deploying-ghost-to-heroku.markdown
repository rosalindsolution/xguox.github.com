---
layout: post
title: "Deploying Ghost to Heroku"
date: 2013-10-18 13:00
comments: true
categories: Ghost Heroku
---

原本没有意思要班门弄斧写什么Ghost的部署感受帖的,不过前些天看到@lucifr 的这条tweet,于是还是忍不住写了,就让更多假死Ghost诞生吧.

<blockquote class="twitter-tweet"><p>ghost 一出，网上又要冒出一大堆主题风格相似的 blog 了，然后更新两篇使用感受就处于假死状态</p>&mdash; Lucifr (@lucifr) <a href="https://twitter.com/lucifr/statuses/390104155152343040">October 15, 2013</a></blockquote>
<script async src="//platform.twitter.com/widgets.js" charset="utf-8"></script>

先来感受帖,主要是与Octopress之流相比:  
毕竟还是新东西, bug,issue还有略多没解决的, 功能上也比较欠缺.不过好的地方在于这个移动支持好,  就算在野外拿着移动设备也是可以杠杠的写东西, 只是有多少人如此热衷到必须在室外用移动设备来写呢?还一个是数据的导入导出很简便. 


因为以前玩过Heroku, 所以不用各种安装[Heroku Toolbelt](https://toolbelt.heroku.com/),配置SSH key什么的. 也玩过Node, 好吧,这个也省了. (好吧, 我是不是可以不用写下去了 =.= ) 其实主要只是写写这个过程遇到的一些问题吧. 系统是 OS X 10.8.5

在Local安装Ghost(当前最新版本为0.3.2)  

[下载](https://en.ghost.org/download/) 源代码并解压, cd 过去, 在Node和NPM安装配置没问题的前提下
`npm install`
在本地跑起来
`npm start`
基本没报错就往后走不折腾, 也可以在浏览器玩儿一会先的. 

接着是让Heroku知道怎么把Ghost跑起来, 执行
`echo "web: node index.js" > Procfile`
在Ghost目录 创建`Procfile`文本文件并写入`web: node index.js`

部署到Heroku用的数据库是PostgreSQL, 而不是Ghost默认的sqlite, 所以要先在
`package.json`这个文件里头做些修改, 在`dependencies `节点把sqlite的那行去掉改为

`"pg": "latest",`

别忘了刚开始在本地跑起来的时候已经安装了sqlite3
所以,还要执行
`npm uninstall sqlite3`

下面的一篇参考里头没有提到这点.

准备就绪后就是初始化Git仓库了
```
git init
git add .
git commit -m "Init"
```

本地有了Git仓库,还要有个Heroku的远程库,创建项目
`heroku create`
执行完之后在Heroku的Dashboard看到你的项目了,不过是啥都没,因为还没有push.
生成的项目一般都是类似这样名字很帅的
```
Creating fathomless-reaches-9699... done, stack is cedar
http://fathomless-reaches-9699.herokuapp.com/ | git@heroku.com:fathomless-reaches-9699.git
```

配置数据库之前先装个PostgreSQL的插件.

`heroku addons:add heroku-postgresql:dev`

该命令会输出`Attached as HEROKU_POSTGRESQL_BLACK_URL....` 类似的信息,其中的BLACK是随机的颜色单词. 下一步命令要用到的.

`heroku pg:promote HEROKU_POSTGRESQL_BLACK`

插件安装就搞定了,接下来是配置文件. 编辑根目录下的`config.js`

主要要改的地方都在development下, 没发觉不同模式的区别, 如果发图片是用上传的方式的话,在每次push之后图片都会没掉,所以还是用外链好了, 用production也一样,记得改环境变量`
heroku config:set NODE_ENV=production`

1 url
比如这里改成`   url: 'http://fathomless-reaches-9699.herokuapp.com',`
2 database
这里要先登录到你的Heroku的Dashboard, 在你的app下可以看到插件栏的`Heroku Postgres Dev :: [Color]`,点进去就看到你的数据库的各种配置信息,如下图:

![](https://g3axqg.dm2301.livefilestore.com/y2pAXIXhAaAoKzlQnO0BMJtfAxFJpGGYjlu_qbNuISsChkrL3vj1grBXJW0RDIbw1kZPze2If1ZX_jQDNy9SDU-8RpPHYc94USB54Mx7Dc6r04/QQ20131018-3.png?psid=1)

![](https://g3yyhw.dm2302.livefilestore.com/y2pKkKpTuU-QBtu3ZzKEfcZGPiHuH_oRCDn1dBt7N0wCh9uLkSGkqdjbc8f09uPvejIoGEOoUdn-IQICWWQVUCIvI3ZGdECSNfyRgXtd4JrTRo/QQ20131018-4.png?psid=1)

相应的把`config.js` development下的database修改成如下:
```
database: {
        url: 'http://fathomless-reaches-9699.herokuapp.com/',
        mail: {},
        database: {
            client: 'postgres',
            connection: {
                host: 'ec2-54-235-92-161.compute-1.amazonaws.com',
                user: 'tgbutpeekudvst',
                password: 'POpn0EK7TNfW1nfgVDsXgw_N4L',
                database: 'd3399k1nh326po',
                port: '5432'
            },
            debug: false
        },
```
3 server

将原本的
```
        server: {
               host: '127.0.0.1',
               port: '2368'
        }
```

```
        server: {
            host: '0.0.0.0',
            port: process.env.PORT
        }
```

然后就可以提交准备push了

```
 git add .
 git commit -m 'for push'
```
最后Push到Heroku
`git push heroku master`  
其实, 后面才是我要写的点. 当我执行这条命令时,
不断报错如下:
```
Received disconnect from 50.19.85.132: 10: user closed connection
error: pack-objects died of signal 13
error: failed to push some refs to 'git@heroku.com:fathomless-reaches-9699.git'
```

话说表示至今没找着原因和解决方法.
求救于Google和StackOverflow的大神也不行. 什么`git repack`的不起作用. 后来发了个ticket给heroku求助.貌似没鸟我,但是第二天忽然就好了,能push了.然后前一秒push出错,没过一会再push又成功了.

另外的还有一个地方下面的参考没有提到的是, 成功部署上去后打开链接可能你会看到这样的错误信息

```
An error occurred in the application and your page could not be served. Please try again in a few moments.

If you are the application owner, check your logs for details.
```

这东西也纠结了我老久,Logs显示
`ERROR: Invalid value, check page, pages, limit and total are numbers`  
没找着原因,后来在官方的Forum上看到的解决办法,是把`content/themes/casper/index.hbs`里头的 {% raw %} {{pagination}} {% endraw %}注释掉.

然后,就没然后了, 部署完毕,打开你的项目链接玩儿Ghost吧.

另外试过用Mysql作为数据库,但是因为Heroku上唯一的Mysql插件[ClearDB]()用了之后会有权限[BUG](https://github.com/TryGhost/Ghost/issues/863),貌似官方还没解决. 所以还是老老实实的用了PostgreSQL

主题的话,[这里](http://marketplace.ghost.org/)有收费和免费的可以下载, 下载下来copy到`content/themes/`这个目录下就OK了, push之后就可以在你的设置页面里直接更换了. 好吧,这样的话"一大堆主题风格相似的 blog"就不能出现了啊.

另, 评论也需要另外添加设置, easy job啦. [参考这里](http://christophvoigt.com/enable-comments-on-ghost-with-disqus/)

参考资源:

[http://docs.ghost.org/installation/](http://docs.ghost.org/installation/)  
[http://www.howtoinstallghost.com/how-to-install-ghost-on-heroku/](http://www.howtoinstallghost.com/how-to-install-ghost-on-heroku/)  
[http://d5c.me/deploying-ghost-to-heroku/](http://d5c.me/deploying-ghost-to-heroku/)
