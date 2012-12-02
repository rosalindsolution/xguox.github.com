---
layout: post
title: "Rails中try的用法"
date: 2012-04-20 23:17
comments: true
categories: Script
---

居然今晚才发现Rails中try的用法，初一看，怎么那么像印象中某些语言的异常处理？不对啊，ruby中的异常处理不是长这个样子的。
查了下rails的API才了解了大致用法。

API：

不使用try的时候是<!-- more -->
{% codeblock %}
@person && @person.name
{% endcodeblock %}
需确保@person不是nil，否则会报错
{% codeblock %}
NoMethodError: undefined method `name' for nil:NilClass
{% endcodeblock %}
如果用try的话
{% codeblock %}
@person.try(:name)
{% endcodeblock %}
如果@person为nil则返回nil而不是报错

另外，try还可以接收block作为参数，
{% codeblock %}
Person.try(:find, 1)
@people.try(:collect) {|p| p.name}
{% endcodeblock %}
漏了一句，API开头就说了，像常规的ruby的Object#send 那样工作
看了下源码，貌似是利用了__send__实现的。最近在研究ruby的元编程。