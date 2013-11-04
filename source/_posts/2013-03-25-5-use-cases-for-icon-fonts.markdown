---
layout: post
title: "5 Use Cases for Icon Fonts"
date: 2013-03-25 15:48
comments: true
categories: Translation CSS
---
Icon fonts非常的强大:

- You can CSS the crap out of them and they don't mind.
- 在任何的显示器和分辨率上看起来都很不错
- 无论多大的icons集都只需要一次HTTP请求

下面来看看5个典型的icon fonts用例.

##在开始之前
为了让代码保持整洁和高可读性, 我们将不会用到浏览器前缀. 我们会用到的东西诸如animations, transitions, and transforms, 所有的这些为了确保更好的浏览器支持一般都需要添加hack前缀. 在这里你想如何处理浏览器前缀问题[由你自己决定](http://css-tricks.com/how-to-deal-with-vendor-prefixes/).  
<!-- more -->
正如你在例子中可以看到的, 我使用的是icon fonts托管服务[weloveiconfonts.com](http://weloveiconfonts.com/). 这是一个我自己的项目--原本想写一篇文章关于icon-fonts, 搜索发现没有免费的icon fonts托管服务, 于是就新建了这么个项目来完成这篇文章. 

##1. CSS Loader
现今有许多[很棒的](http://codepen.io/stuffit/pen/vlfyA), [纯](http://codepen.io/FWeinb/pen/BeJLo)CSS Loader, 你也可以在[cssload.net](http://cssload.net/)上创建你自己的Loader. 而除此之外我们还可以使用[icon fonts](http://css-tricks.com/flat-icons-icon-fonts/)来完成. 你可以使用一个看起来像Loading的符号,然后用CSS让其[动起来](http://docs.webplatform.org/wiki/css/properties/animation/animation). 

以下这是基本的HTML. 开始是三个loader, 他们都是`div.wrapper`的子元素. 第四个(`div.complex`)是由两个icons组成.

```html
<div class="wrapper">
  <span class="fontelico-spin1"></span>
  <span class="fontelico-spin3"></span>
  <span class="fontelico-spin5"></span>
</div>

<div class="complex">
  <span class="fontelico-emo-devil"></span>
  <span class="fontelico-spin4"></span>
</div>
```
导入icon font [Fontelico](https://github.com/fontello/fontelico.font)到你的CSS之中.

```css
@import url(http://weloveiconfonts.com/api/?family=fontelico);
[class*="fontelico-"]:before {
  font-family: 'fontelico', sans-serif;
}
```
给元素添加名为`load`的动画, 使得这些元素360度旋转, 可适当做一些调整或者更改颜色. 

```css
/* Normal (left) */
span {
  float: left;
  text-align: center;
  margin: 0 5em;
  animation: load 1.337s infinite ease-out reverse;
}
/* Fast (center) */
.wrapper span:nth-child(2) {
  animation: load .5s infinite linear;
}
/* Steps (right) */
.wrapper span:nth-child(3) {
  animation: load 1.25s infinite steps(18, end) forwards;
}
@keyframes load {
  0% {
    transform: rotate(0) scale(1, 1);
    color: rgba(0, 0, 0, .5);
  }
  10% { color: rgba(0, 120, 0, .5); }
  20% { color: rgba(0, 120, 120, .5); }
  30% { color: rgba(120, 120, 0, .5); }
  40% { color: rgba(0, 0, 120, .5); }
  50% {
    transform: rotate(180deg) scale(1.85, 1.85);
    color: rgba(120, 0, 0, .5);
  }
  100% {
    transform: rotate(360deg) scale(1, 1);
    color: rgba(0, 0, 0, .5);
  }
}
```
将最后的那个loader(`div.complex`) 的两个icons堆叠在最上层. 第一个子元素为devil形状的icon(用的是名为`rotate`的动画), 第二个子元素为spin icon(使用名为`scale`的动画). 

```css
.complex span:nth-child(1),
.complex span:nth-child(2) {
  position: absolute;
  margin: 0;  
  width: 1em;
  height: 1em;
}
/* Devil icon  */
.complex span:nth-child(1) {
  animation: load 1.25s infinite steps(18, end) forwards;
}
/* Spin icon */
.complex span:nth-child(2) {
  font-size: 3em;
  left: -.35em;
  top: -.35em;
  color: rgba(0, 0, 0, .3);
  animation: devil 3s infinite linear reverse forwards;
}
@keyframes devil {
  0% {
    transform: scale(-1.85, 1.85);
  }
  50% {
    transform: scale(1.85, -1.85);
  }
  100% {
    transform: scale(-1.85, 1.85);
  }
}
```
{% codepen FCsHe TimPietrusky %}

##SocialCount meets Style
人们都喜欢在他们的网站上使用一些社交分享按钮, 从而使得访问者可以轻易的传播他们的内容. 但是当你如果添加Facebook, Twitter, Google+, 那么你将要面临的是大约309KB的空缓存负载量. 这就是[SocialCount jQuery plugin](https://github.com/filamentgroup/SocialCount)所存在的原因. SocialCount是渐进增强的, 并且可以lazy loaded以及对移动设备比较友好.

它的默认样式很简单且在iframe中包含的buttons也很少. 下面, 让我们为这些buttons添加一些CSS花样. 

以下的HTML是由[SocialCount markup generator](SocialCount markup generator)所创建. 其实也就是一些包含着链接和icons的无序列表. 

```html
<ul
  class="socialcount"
  data-url="http://www.google.com/"
  data-counts="true"
  data-share-text="Google is a search engine">
 
  <li class="facebook">
    <a href="https://www.facebook.com/sharer/sharer.php?u=http://www.google.com/" title="Share on Facebook">
      <span class="count entypo-thumbs-up"></span>
    </a>
  </li>

  <li class="twitter">
    <a href="https://twitter.com/intent/tweet?text=http://www.google.com/" title="Share on Twitter">
      <span class="count entypo-twitter"></span>
    </a>
  </li>

  <li class="googleplus">
    <a href="https://plus.google.com/share?url=http://www.google.com/" title="Share on Google Plus">
        <span class="count entypo-gplus"></span>
    </a>
  </li>

</ul>
```
接着在CSS中导入icon font(Entypo)

```css
@import url(http://weloveiconfonts.com/api/?family=entypo);
[class*="entypo-"]:before {
  font: 2.5em/1.9em 'entypo', sans-serif;
}
```
然后是重写SocialCount提供的默认样式:

- 加入transition
- 缩放iframe
- 给buttons设定自定义的背景

> Editor's note: 以下的CSS所使用的是SASS的一种格式SCSS.  包括后边的也是. 如果你想要使用标准的CSS, 可以跳转到下边的CodePen 演示链接,  在CSS编辑框头部点击"SCSS", 然后就可以看到经过编译后的标准CSS

```sass
.socialcount {
  > li {
    width: 33%;
    border-radius: 0;
    transition: all .3s ease-in-out;
    cursor: pointer;
   
    &:hover [class*="entypo-"]:before {
      opacity: 0;  
    }
  }
 
  iframe {
    transform: scale(1.65, 1.65);    
  }
 
  .button {
    top: 50%;
    margin: -.75em 0 0 0;
    height: 2em;
  }
 
  .facebook {
    background: rgba(59, 89, 152, .7);
  }
   
  &.like .facebook iframe {
    width: 8em;
  }
 
  .twitter {
    background: rgba(0, 172, 237, .7);  
  }
 
  .googleplus {
    background: rgba(172, 0, 0, .7);  
  }
}
```
{% codepen Czfyu TimPietrusky %}

##3.Enhanced lists
下边的三个例子都基于下边这个简单的HTML

```html
<div>
  <h2>Title</h2>
  <ul class="style">
    <li data-text="">Text</li>
    <li data-text="">Text</li>
    <li data-text="">Text</li>
  </ul>
</div>
```
导入 icon font (Font Awesome)到你的CSS

```css
@import url(http://weloveiconfonts.com/api/?family=fontawesome);
[class*="fontawesome-"]:before {
  font-family: 'FontAwesome', sans-serif;
}
```
为列表, 列表元素, [伪元素](http://css-tricks.com/almanac/selectors/a/after/)以及button添加默认样式. 在这里我们不使用class来设定列表元素下的icon的样式, 取而代之是我们改变元素的`content`属性来创建一个伪元素. 

```css
ul {
  list-style: none;
  padding: 0;
   
  li {
    position: relative;
    min-height: 2em;
    padding: .3em .3em .3em 1.5em;
    transition:
      background .3s ease-in-out,
      color .3s ease-in-out,
      box-shadow .1s ease-in-out,
      height .25s ease-in-out
   ;
   
    button {
      position: absolute;
      left: 1.45em;
      bottom: .35em;
      opacity: 0;
      height: 0;
      border: none;
      font-size: .8em;
      cursor: pointer;
      transition: all .4s ease-in-out;
    }
   
    &:before {
      position: absolute;
      top: .425em;
      font-family: 'FontAwesome', sans-serif;
      margin: 0 .35em 0 -1.35em;
      vertical-align: bottom;
    }
    &:hover {
      button {
        opacity: 1;
        height: 2em;
      }
     
      &:before {
        color: rgba(255, 255, 255, 1);
        right: 0;
        transform: scale(2.5, 2.5) translate(-.25em, .15em);
      }
      &:after {
        position: absolute;
        content: attr(data-text);
      }
    }
  }
}
```
###3.1 Stuff You Love
第一个示例很简单. 使用的是一个心形而不是默认的列表样式加特定的hover: icon会被移到右侧, 另外`::after`伪元素得到data-text这个[data-attribute](http://www.whatwg.org/specs/web-apps/current-work/multipage/elements.html#embedding-custom-non-visible-data-with-the-data-*-attributes)的文本

```css
.love {
  li {  
    &:before {
      /* fontawesome-heart */
      content: "\f004";
    }
    &:before,
    &:after {
      color: rgba(220, 20, 20, .6);
    }
    &:hover {
      background: rgba(220, 20, 20, .6);
   
      &:after {
        width: 2em;
        right: .445em;
      }
    }
  }
}
```
###3.2 Downloads
这是一个自定义列表下载扩展标记: 每个列表元素有一个链接和一个icon. 按钮被隐藏了, 只有当hover的时候才显示. 

```css
.downloads {
  li {
    border-bottom: .15em solid rgba(0, 0, 0, .3);
   
    &:before {
      /* fontawesome-download-alt */
      content: "\f019";
      color: rgba(50, 50, 50, .5);
    }
    &:hover {
      background: rgba(0, 140, 0, .7);
      height: 4em;
   
      &:after {
        right: .35em;
      }
     
      &:before {
        color: rgba(255, 255, 255, .2);
      }
    }
  }
}
```
###3.3 Your account
用户菜单的UI. 当你hover在一个列表元素上的时候, icon移动到右侧并且box-shadow填满整个背景. 基于`:nth-child`选择器和`content`属性, 每个列表元素都有一个自己的icon.

```css
.account {
  padding: .75em;
  border: .15em solid rgba(0, 0, 0, .2);
  background-color: rgba(0, 0, 0, .7);
  background-image: radial-gradient(center, ellipse cover, rgba(104,104,104,0.6) 0%,rgba(23,23,23,0.7) 100%);
  box-shadow: inset 0 0 .35em 0 rgba(0, 0, 0, .2);
  color: rgba(255, 255, 255, .6);
  backface-visibility: hidden;
   
  li {  
    cursor: pointer;
       
    &:nth-child(1):before {
      /* fontawesome-heart-empty */
      content: "\f08a";
    }
    &:nth-child(2):before {
      /* fontawesome-glass */
      content: "\f000";
    }
    &:nth-child(3) {
      margin-bottom: 1em;
     
      &:before {
      /* fontawesome-comment */
        content: "\f075";
      }
    }
    &:nth-child(4) {
      margin-bottom: .5em;
     
      &:before {
      /* fontawesome-cog */
         content: "\f013";
      }
    }
    &:nth-child(5):before {
      /* fontawesome-signout */
      content: "\f08b";
    }
    &:hover {
      color: rgba(255, 255, 255, 1);
      padding-left: 1.5em;
      box-shadow: inset 0 0 0 10em rgba(255, 255, 255, .5);
   
      &:before {
        color: rgba(255, 255, 255, 1);
        transform: none;
      }  
    }
  }
}
```
{% codepen tcklJ TimPietrusky %}
##4. Emocons.js jQuery Plugin
[Emojons.js](https://github.com/TimPietrusky/emocons)是一个搜索元素里特定字符序列内容的jQuery插件. 所有匹配上的都会用一个`span`和相应的icon HTML class来替换.  
例如, 一个聊天app:

```html
<div class="chat">
    :D :) ;) :'( :o :/ :( B) :P :|
    :beer: :devil: :shoot: :coffee: :thumbsup: :angry: :ueber-happy:
</div>
```
接着在你的元素上调用 Emocons.js 插件的方法

```javascript
$('.chat').emocons();
```
现在, 所有在聊天时候的元素都会被转换成这样:

```html
<span class="fontelico-emo-grin go" title=":D"></span>
```
在CSS中导入icon font(Fontelico):

```css
@import url(http://weloveiconfonts.com/api/?family=fontelico);
[class*="fontelico-"]:before {
  font-family: 'fontelico', sans-serif;
}
```
当 Emocons.js 把一个字符串转换成表情符号时, 会给`span`元素增加一个`go`类(添加一个动画效果).

```css
.go {
  animation: hey .55s 1 linear;
}
@keyframes hey {
  0% {
    transform: scale(1, 1);
  }
  50% {
    transform: scale(1.85, -1.85);
  }
  100% {
    transform: scale(1, 1);
  }
}
```
一些表情符号会有特殊的样式(如颜色的改变)或者通过`::after`伪元素添加一些东西

```css
.fontelico-emo-devil {
  color:rgba(180, 0, 0, .9);
}
.fontelico-emo-beer:after {
  box-shadow:
    -.475em .75em 0 .275em rgba(220, 220, 0, 1),
    -.185em .675em 0 .175em rgba(220, 220, 0, 1)
  ;    
}
.fontelico-emo-coffee:after {
  box-shadow:
    -.475em .78em 0 .235em rgba(222, 184, 135, 1),
    -.215em .715em 0 .155em rgba(222, 184, 135, 1)
  ;    
}
.fontelico-emo-shoot:after {
  border-radius: .15em;
  box-shadow: .315em .525em 0 .1em rgba(0, 0, 0, 1);    
}
.fontelico-emo-angry:after {
  border-radius: .15em;
  box-shadow: -.695em .455em 0 .085em rgba(0, 220, 0, .6);
  z-index: 2;
}
```
{% codepen KloGD TimPietrusky %}
##5. Parallax Movie #1337
最后的这个例子只是为了好玩吧. 它只是一个试验, 使用一个icon font来创建一个无穷的parallax movie. 创建场景的HTML包含有许多的元素. 

- The setting: sky, ground, night and sun
- Fixed element: bicycle
- Animated elements: Trees, giraffe, shopping cart, buildings and heliport

```html
<div class="night"></div>
<div class="wrapper">
  <div class="sun">
    <div class="maki-fast-food"></div>
  </div>
 
  <div class="sky"></div>
  <span class="maki-bicycle"></span>
   
  <span class="maki-tree-1" data-child="1"></span>
  <span class="maki-tree-1" data-child="2"></span>
  <span class="maki-tree-1" data-child="3"></span>
   
  <span class="maki-giraffe"></span>
  <span class="maki-grocery-store"></span>
   
  <span class="maki-commerical-building" data-child="1"></span>
  <span class="maki-commerical-building" data-child="2"></span>
   
  <span class="maki-heliport"></span>
   
  <div class="ground"></div>
</div>
```
在CSS中导入 icon font(Maki):

```css
@import url(http://weloveiconfonts.com/api/?family=maki);
[class*="maki-"] {
  position: absolute;
  margin: 0;  
  color: #fff;
  font-size: 2em;
}
[class*="maki-"]:before{
  font-family: 'maki', sans-serif;
}
*:after {
  position: absolute;
  top: 0;
  right: 0;
  content: '';
  z-index: -1;
  width: 0;
  height: 0;
}
```
整个场景旋转 "-3度".

```css
.wrapper {
  height: 140%;
  width: 120%;
  transform: rotate(-3deg) translate(-10%, -15%);    
}
```
当日落西山后, 场景变换为夜晚

```css
.night {
  position: absolute;
  z-index: 5;
  width: 100%;
  height: 100%;
  animation: night 45s infinite forwards;
}
@keyframes night {
  0%, 30%, 100% {background:rgba(0, 0, 0, 0);}  
  55% {background: rgba(0, 0, 0, .6);}
}
```
sky与ground都用的是同样的`background-position`动画, 只是速度有所不同. 

```css
.sky {
  position: relative;
  z-index: 0;
  background: url(http://subtlepatterns.subtlepatterns.netdna-cdn.com/patterns/bedge_grunge.png);
  height: 50%;    
  width: 100%;
  animation: rollin-bg 25s linear infinite forwards;
}
.ground {
  position: absolute;
  z-index: 1;
  background: url(http://subtlepatterns.subtlepatterns.netdna-cdn.com/patterns/blackorchid.png);
  height: 50%;    
  width: 100%;
  animation: rollin-bg 7s linear infinite forwards;
}
@keyframes rollin-bg {
  0% { background-position: 100%; }  
  100% { background-position: 0; }
}
```
[沿着圆周移动一个元素](http://lea.verou.me/2012/02/moving-an-element-along-a-circle/)(the burger-sun)

```css
.sun {
  position: absolute;
  z-index: 1;
  left: 50%;
  top: 10%;
  width: 2em;
  height: 2em;
  font-size: 4em;
  line-height: 1;
  animation: circle 45s linear infinite;
  transform-origin: 50% 3.85em;
}
.sun [class*="maki-"] {
  color: rgba(240, 180, 0, .2);
  text-shadow: 0 0 .35em rgba(240, 240, 0, .7);
}
.sun > div {
  animation: inner-circle 45s linear infinite;
}
@keyframes circle {
  from { transform: rotate(0deg); }
  to { transform: rotate(360deg); }
}
@keyframes inner-circle {
  from { transform: rotate(0deg); }
  to { transform: rotate(-360deg); }
}
```

以下这个是唯一一个没有使用动画效果的元素, 但它看起来还是在移动.

```css
.maki-bicycle {
  left: 50%;
  z-index: 4;
  margin: -.85em 0 0 -.5em;
  color: rgba(30, 30, 140, .95);
}
```
这里全都是使用同样的滚动动画, 但速度不一的元素.

```css
.maki-tree-1[data-child="1"] {
  margin: -1em 0 0 5%;
  z-index: 5;
  animation: rollin 5s linear infinite;
  font-size: 2.4em;
  color: rgba(0, 110, 0, .8);
}
.maki-tree-1[data-child="2"] {
  margin: -1em 0 0 85%;  
  z-index: 2;
  animation: rollin 12s linear infinite;
  font-size: 1.6em;
  color: rgba(0, 110, 0, .5);
}
.maki-tree-1[data-child="3"] {
  margin: -1em 0 0 25%;  
  z-index: 2;
  animation: rollin 17s linear infinite;
  font-size: 1.2em;
  color: rgba(0, 110, 0, .3);
}
.maki-giraffe {
  margin: .25em 0 0 5%;  
  z-index: 6;
  animation: rollin 12s linear infinite reverse;
  font-size: 10em;
  color: rgba(255, 255, 10, .9);
}
.maki-giraffe:after {
  right: -3em;
  content: '\e82a \e82a \e82a \e82a \e82a';
  font: .2em 'Maki', sans-serif;
  letter-spacing: .2em;
  width: 3em;
  color: rgba(0, 0, 0, .6);
  box-shadow:
    0 .45em 0 .75em rgba(255, 255, 255, .4),
    1em .35em 0 .75em rgba(255, 255, 255, .4),
    2.25em .25em 0 1.05em rgba(255, 255, 255, .4)  
  ;
  border-radius: 50%;
  transform: translate(2.3em, .55em) rotateY(-180deg);
}
.maki-grocery-store {
  margin: -.35em 0 0 5%;
  z-index: 5;
  animation: rollin 22s linear infinite;
  font-size: 4em;
  color: rgba(220, 0, 10, .7);
}
.maki-commerical-building[data-child="1"] {
  margin: -1em 0 0 5%;
  z-index: 3;
  animation: rollin 6s linear infinite;
  font-size: 7em;
  color: rgba(120, 0, 120, .8);
}
.maki-commerical-building[data-child="2"] {
  margin: -1em 0 0 5%;
  z-index: 2;
  animation: rollin 14s linear infinite;
  font-size: 4em;
  color: rgba(0, 120, 120, .4);
}
.maki-heliport {
  margin: -3.5em 0 0 2em;  
  z-index: 1;
  color: rgba(30, 30, 30, .45);
  font-size: 1.25em;
  animation: rollin 26s linear infinite reverse 2s;
}
@keyframes rollin {
  0% {margin-left:105%}  
  100% {margin-left:-15%;}
}
```
{% codepen ELuiG TimPietrusky %}


原文来自 Tim Pietrusky [5 Use Cases for Icon Fonts](http://css-tricks.com/five-use-cases-for-icon-fonts/)