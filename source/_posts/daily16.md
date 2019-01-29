---
title: 日常问题解决（持续更新中）
date: 2019-1-2 21:06:28
tags: 其他
categories: 个人
---

### 解决使用 swiper 常见的问题
**1、swiper近视初始化时, 其父级元素处于隐藏状态(display:none),会导致swiper初始化失败, 页面中的滚动效果有问题**
```

解决方法1: 
 var mySwiper = new Swiper('.demo',{
     observer: true,//修改swiper自己或子元素时，自动初始化swiper
     observeParents: true//修改swiper的父元素时，自动初始化swiper
 });
 
解决方法2: 
固定宽和高
  var mySwiper = new Swiper('.demo',{
     width:200,
     height:200
 });
 ```
**2、swiper里面的图片懒加载与预加载, 可以使用自带的 lazyload 方法**
[主要看这里-swiper4 懒加载文档](https://www.swiper.com.cn/api/lazy/213.html)

```
设为true开启图片延迟加载默认值，使preloadImages无效。或者设置延迟加载选项。
 
图片延迟加载：需要将图片img标签的src改写成data-src，并且增加类名swiper-lazy。
背景图延迟加载：载体增加属性data-background，并且增加类名swiper-lazy。
 
还可以加一个预加载，<div class="swiper-lazy-preloader"></div>
或者白色的<div class="swiper-lazy-preloader swiper-lazy-preloader-white"></div>
 
当你设置了slidesPerView:'auto' 或者 slidesPerView > 1，还需要开启watchSlidesVisibility。
 
 
var mySwiper = new Swiper('.swiper-container', {
  lazy: {
    loadPrevNext: true,
  },
});

```
**3、想在轮播图外创建分页器、上一页和下一页的按钮(因为swiper的container默认overflow:hidden, 只能在轮播图中的可视区域显示切换菜单和上一页下一页)**
```
var mySwiper = new Swiper('.swiper-container',{
    pagination : '.swiper-pagination',
    uniqueNavElements :false,
})
```
### jquery fullpage 插件增加头部和底部的方法
官方给出了解决方案，不要去什么修改源码啦之类的，或者自己写代码判断啦
```
<div class="fullpage">
    <div class="section fp-auto-height">这里写头部</div>
    <div class="section">page1</div>
    <div class="section">page2</div>
    <div class="section">page3</div>
    <div class="section">page4</div>
    <div class="section fp-auto-height">这里写版权</div>
</div>
```
如上，js代码就不说了，只要你能跑起来，就没有问题。这里只需要给头部和底部增加一个fp-auto-height 的 class ，然后自己可以加点class写样式。

没有生效吗？

嘿嘿，那是因为你只引用了js，而没有引用css造成的，只要引用下面的css即可。

https://github.com/alvarotrigo/fullPage.js/blob/master/dist/jquery.fullpage.css

其实关键代码只是下面的而已
```
    .fp-auto-height.fp-section,
    .fp-auto-height .fp-slide,
    .fp-auto-height .fp-tableCell{
        height: auto !important;
    }

    .fp-responsive .fp-auto-height-responsive.fp-section,
    .fp-responsive .fp-auto-height-responsive .fp-slide,
    .fp-responsive .fp-auto-height-responsive .fp-tableCell {
        height: auto !important;
    }
```
其他参数配置见[官网](http://fullpage.81hu.com/)

### 对于多行text-overflow:ellipsis 溢出显示省略号的解决办法（尽量兼容所有浏览器）
**1、css解决**
```
@mixin ellipsis($line:1){
  word-break: break-all;
  @if $line == 1{
    overflow: hidden;
    text-overflow: ellipsis;
    white-space: nowrap;
  }@else{
    text-overflow:clip;
    display: -webkit-box;
    overflow: hidden;
    word-wrap: break-word;
    white-space: normal !important;
    -webkit-line-clamp:$line;
    -webkit-box-orient: vertical;
  }
}
```
**2、js解决**
```
//截取的函数 双字节字符长度为2 ASCLL字符长度为1
function cutStr(str,cutLen){
    var returnStr = '',    //返回的字符串
        reCN = /[^\x00-\xff]/,    //双字节字符
        strCNLen = str.replace(/[^\x00-\xff]/g,'**').length; 
    if(cutLen>=strCNLen){
        return str;
    }
    for(var i=0,len=0;len<cutLen;i++){
        returnStr += str.charAt(i);
        if(reCN.test(str.charAt(i))){
            len+=2;
        }else{
            len++;
        }
    }
    return returnStr;
}
$(function (){
    var str = $(this).text();
    $(this).text(cutStr(str,34)+'...');  
});

```
**3、哈哈哈哈，后台解决**
麻烦以下后端小伙伴帮忙处理以下

后续继续整理
