---
title: viewport-fit解决iPhone X、XS、XS Max、XR刘海屏问题
date: 2018-10-10 11:28:29
tags: 移动端
categories: 个人
---

#### 一次活动页面发现的iPhone X、XS、XS Max、XR刘海屏问题

起因：游戏内嵌内嵌H5页面，提供的webview容器是全屏的，所以H5页面要处理以上设备的刘海问题【烦】。

尺寸了解我这里就不写了，尺寸问题我就不写了，[顶楼电梯](https://blog.csdn.net/qq_33608748/article/details/82769570)

iPhone X 配备一个覆盖整个手机的全面屏,顶部的“刘海”突出来使得网站被限制在一个“安全区域”,在两侧边缘会出现白条儿。移除这个白条儿也不难,给 body 设置一个 background-color 就可以搞定。

但是我们内嵌的游戏页面背景色有时候不好设置背景色为纯色，另一种方法就是添加 viewport-fit=cover meta 标签,将整个网站扩展到整个屏幕

```
<meta name="viewport" content="width=device-width, initial-scale=1.0, viewport-fit=cover">
```

iPhone XS等手机还未发布之前，我们也是跟着学做已经做一些兼容来搞定iPhoneX。

这里只是横屏代码，竖屏同理；

```
var isIPhoneX = /iphone/gi.test(window.navigator.userAgent) && window.devicePixelRatio && window.devicePixelRatio === 3 && window.screen.height === 375 && window.screen.width === 812;
```

这里我们判断如果是iPhone X，把顶部增加高度30px的容器垫高，避开刘海头区域，各种方式都可以padding-left、absolute、margin-left。。。你能想到的方法都可以，这样标题正好避开刘海头。

因为自己当时还没有用iPhone X，只知道iPhone X有刘海头，不知道其他细节问题。

又到一年一度的9月份，苹果发布了3X机系列，有同事在Mac下的iPhone模拟器访问，发现这个页面iPhone XS Max下有问题。看了一下上面文章发现尺寸不一样，当初只判断了iPhone X加垫高，其他几个机型都未判断，所以就很自然的写了新机型，加上判断：

```
// iPhone X、iPhone XS
var isIPhoneX = /iphone/gi.test(window.navigator.userAgent) && window.devicePixelRatio && window.devicePixelRatio === 3 && window.screen.height === 375 && window.screen.width === 812;
// iPhone XS Max
var isIPhoneXSMax = /iphone/gi.test(window.navigator.userAgent) && window.devicePixelRatio && window.devicePixelRatio === 3 && window.screen.height === 414 && window.screen.width === 896;
// iPhone XR
var isIPhoneXR = /iphone/gi.test(window.navigator.userAgent) && window.devicePixelRatio && window.devicePixelRatio === 2 && window.screen.height === 414 && window.screen.width === 896;
```

原来代码是if(isIPhoneX)垫高，现在改成if(isIPhoneX || isIPhoneXSMax || isIPhoneXR)垫高。

这里有个坑，官方提供的安全区域代码constant(safe-area-inset-top) env(safe-area-inset-top)，使用后都在刘海头下面，如图（黑灰色区域状态栏）：

![](http://ons.me/wp-content/uploads/2018/10/4.jpg)

当初没有用安全区域代码，一方面是因为页面有悬浮容器，会悬浮到安全区域外部，兼容页面正文麻烦，另一方面就是正文内容靠下，离刘海头有一段距离，感觉太丑干脆不用。

总结：依旧不用安全区域代码，如果要做刘海头，if(isIPhoneX || isIPhoneXSMax || isIPhoneXR) 垫高44px。

备注：iPhone X、iPhone XS、iPhone XS Max刘海头高度30px，iPhone XR刘海头高度33px。本文提到的30px、33px、44px，均为initial-scale=1下，px是在通用属性下，用rem写页面的请自行转换。设计稿像素应该都需要乘以2倍或3倍。