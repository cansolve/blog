---
title: 常见jquery方法汇总
date: 2019-03-22 10:36:19
tags: 其他
categories: 个人
---
最近前后端分离模式写的也少了，因为公司做的游戏网站都是面向全球的，涉及到ssr，根据业务及人员需求，采用php渲染的方式，于是我这边又回到了一个切图仔的身份，完成布局，写点交互效果就好了，好了不说了，有点惨，这里总结一下常用的jq方法，其实也是为了自己方便查阅，哈哈！！
## jQuery 语法

> $(selector).action()

文档加载就绪事件

```
$(document).ready(function() {
  // 代码...
});

// 简写方式
$(function() {
  // 代码...
});
```

$(document).ready 与 window.onload 的区别

> $(document).ready 和 window.onload 都是在都是在页面加载完执行的函数，大多数情况下差别不大。
>  $(document).ready:是 DOM 结构绘制完毕后就执行，不必等到加载完毕。 意思就是 DOM 树加载完毕，就执行，不必等到页面中图片或其他外部文件都加载完毕。并且可以写多个.ready。
>  window.onload:是页面所有元素都加载完毕，包括图片等所有元素。只能执行一次。

## 选择器

jQuery 选择器基于已经存在的 CSS 选择器

> $('*')
>  ​$('p')
>  $('ul li')
>  ...

## 事件

-   click() 点击事件
    
-   dbclick() 双击事件
    
-   mouseenter() 鼠标穿过元素事件
    
-   mouseleave() 鼠标离开元素事件
    
-   mousedown() 鼠标移动到元素上方按下鼠标事件
    
-   mouseup() 鼠标按住移动到元素上方松开鼠标事件
    
-   hover() 鼠标悬停事件
    
-   focus() 表单元素聚焦事件
    
-   blur() 表单元素失去焦点事件
    
-   submit() 表单提交事件
    
-   change() 表单元素值改变事件
    
-   keypress() 键盘键按住事件
    
-   keydown() 键盘键按下事件
    
-   keyup() 键盘键松开事件
    
-   load() 指定元素加载完成式执行事件 （1.8 版本后废弃）
    
-   resize() 窗口大小改变事件
    
-   scroll() 滚动监听事件
    

## 效果

> $(selector).action(speed,callback)

### 显示隐藏

-   hide() 隐藏元素
    
-   show() 显示元素
    
-   toggle() 显示被隐藏的元素，隐藏已显示的元素
    

### 淡入淡出

-   fadeIn() 淡入
    
-   fadeOut() 淡出
    
-   fadeToggle() 已淡出的元素淡入，已淡入的元素淡出
    
-   fadeTo() 渐变为给定不透明度
    
    > $(selector).fadeTo(speed,opacity,callback);
    > 
    > opacity 值为 0 与 1 之间
    

### 滑动

-   slideDown() 向下滑动展开元素
    
-   slideDown() 向上滑动收起元素
    
-   slideToggle() 已展开元素上滑收起，已收起元素下滑展示
    

### 动画

> $(selector).animate({params},speed,callback);

实例

```
$("button").click(function() {
  $("div").animate({
    left: "250px",
    opacity: "0.5",
    height: "150px",
    width: "150px"
  });
});
```

### 停止动画

> $(selector).stop(stopAll, goToEnd);

## jQuery 链(Chaining)

jQuery支持在一条语句中运行多个 jQuery 方法（在相同的元素上，浏览器就不必多次查找相同的元素。

```
$("#p1")
  .css("color", "red")
  .slideUp(2000)
  .slideDown(2000);

// "p1" 元素首先会变为红色，然后向上滑动，再然后向下滑动
```

### 获取内容

-   text() 设置或返回所选元素的文本内容
    
-   html() 设置或返回所选元素的内容（包括 HTML 标记）
    
-   val() 设置或返回表单字段的值
    

### 获取属性

-   attr() 设置或者返回所选的属性的值
    

```

// 获取属性
$('#test').attr('href'）

// 设置属性
$('#test').attr('href','http://www.baidu.com')

$('#test').attr({
    href: 'http://www.baidu.com',
    title: '百度'
})

// 回掉函数
$('#test').attr('href', function(i, origValue){
    // i 被选元素列表中当前元素的下标
    // origValue 原始值
    return origValue + '/jquery'
  })
```

### 添加删除元素

-   append() 在被选元素的结尾插入内容
    
-   prepend() 在被选元素的开头插入内容
    
-   after() 在被选元素之后插入内容
    
-   before() 在被选元素之前插入内容
    
-   remove() 删除被选元素（及其子元素）
    
-   empty() 从被选元素中删除子元素
    

> $('p').remove('.italic')

### 获取并设置 css 类

-   addClass() 向被选元素添加一个或多个类
    
-   removeClass() 从被选元素删除一个或多个类
    
-   toggleClass() 对被选元素进行添加/删除类的切换操作
    
-   css() 设置或返回样式属性
    

```
// 返回样式属性
$("p").css("background-color");

// 设置样式属性
$("p").css("background-color", "yellow");
// 或者
$("p").css({ "background-color": "yellow", "font-size": "200%" });
```

### 尺寸方法

-   width() 元素宽度
    
-   height() 元素高度
    
-   innerWidth() 包含 padding 宽度
    
-   innerHeight() 包含 padding 高度
    
-   outerWidth() 包含 padding、border 宽度
    
-   outerHeight() 包含 padding、border 高度
    
-   outerWidth(true) 包含 padding、border、margin 宽度
    
-   outerHeight(true) 包含 padding、border、margin 高度
    

### 元素遍历

祖先元素：

-   parent() 返回被选元素的直接父元素，该方法只会向上一级对 DOM 树进行遍历。
    
-   parents() 返回被选元素的所有祖先元素，它一路向上直到文档的根元素 (<html>)。
    
-   parentsUntil() parentsUntil() 方法返回介于两个给定元素之间的所有祖先元素。
    

```
$(document).ready(function() {
  // div > ul > li > span
  $("span").parentsUntil("div");
  // 返回 ul 和 li
});
```

后代元素：

-   children() 返回被选元素的所有直接子元素。
    
-   find() 方法返回被选元素的后代元素，一路向下直到最后一个后代。
    

```
$(document).ready(function() {
  $("div").find("span");
});
```

同胞元素：

-   siblings() 返回被选元素的所有同胞元素。
    
-   next() 返回被选元素的下一个同胞元素。
    
-   nextAll() 返回被选元素的所有跟随的同胞元素。
    
-   nextUntil() 返回介于两个给定参数之间的所有跟随的同胞元素。
    
-   prev() 返回被选元素的上一个同胞元素。
    
-   prevAll() 返回被选元素之前的所有的同胞元素。
    
-   prevUntil() 返回介于两个给定参数之间的所有前方的同胞元素。
    

元素过滤：

-   first() 返回被选元素的首个元素。
    
-   last() 返回被选元素的最后一个元素。
    
-   eq() 返回被选元素中带有指定索引号的元素。
    
-   filter() 方法允许您规定一个标准。不匹配这个标准的元素会被从集合中删除，匹配的元素会被返回。
    
-   not() 方法返回不匹配标准的所有元素。