---
title: 滑向未来（现代 JS 与 CSS 滚动实现指南)
date: 2018-6-3 12:11:23
tags: css
categories: 分享
---

> 英文： Evil Martians   译文：众成翻译/sea_ljf
> www.zcfy.cc/article/scroll-to-the-future

一些（网站）滚动的效果是如此令人着迷但你却不知该如何实现，本文将为你揭开它们的神秘面纱。我们将基于最新的技术与规范为你介绍最新的 JavaScript 与 CSS 特性，（当你付诸实践时，）将使你的页面滚动更平滑、美观且性能更好。

大多数的网页的内容都无法在一屏内全部展现，因而（页面）滚动对于用户而言是必不可少的。对于前端工程师与 UX 设计师而言，跨浏览器提供良好的滚动体验，同时符合设计（要求），无疑是一个挑战。尽管 web 标准的发展速度远超从前，但代码的实现往往是落后的。下文将为你介绍一些常见的关于滚动的案例，检查一下你所用的解决方案是否被更优雅的方案所代替。

**消逝的滚动条**

在过去的三十年里，滚动条的外观不断改变以符合设计的趋势，设计师们为（滚动条的）颜色、阴影、上下箭头的形状与边框的圆角实验了多种风格。以下是 Windows 上的变化历程：

![](https://mmbiz.qpic.cn/mmbiz_png/zPh0erYjkib1S4or5PlS9mBibR4uwkGagfzgsOgAT11uFEPedxAicRlYjePnTImjsvTpKpRnh9MwyTOaCoAU3yq7Q/640?wx_fmt=png&tp=webp&wxfrom=5&wx_lazy=1&wx_co=1)（Windows 上的滚动条）

在2011年，苹果设计师从 ios 上获得灵感，为如何定义“美观的”滚动条确定了方向。所有滚动条均从 Mac 电脑中消失，不再占据任何页面空间，只有在用户触发滚动时（滚动条）才会重新出现（有些用户会设置不隐藏滚动条）。

![](https://mmbiz.qpic.cn/mmbiz_png/zPh0erYjkib1S4or5PlS9mBibR4uwkGagfFRq6PHibZL73jSVzmIrG2gzZ9xW8liaEqR9sYyd5icjOvIz0ianGCFq3JA/640?wx_fmt=png&tp=webp&wxfrom=5&wx_lazy=1&wx_co=1)（Mac 上的滚动条）

滚动条安静地消逝并未引起苹果粉丝的不满，已经习惯了 iPhone 与 iPad 上滚动方式的用户很快地习惯了这一设计。大多数开发人员与设计师都认为这是一个“好消息”，因为计算滚动条的宽度可真是件苦差事。

然而，我们生活在一个拥有众多操作系统与浏览器的世界中，它们（对于滚动）的实现各不相同。如果你和我们一样是一名 Web 开发者，你可不能把“滚动条问题”置之不理。

以下将为你介绍一些小技巧，使你的用户在滚动时有更好的体验。

**隐藏但可滚动**

先来看看一个关于模态框的经典例子。当它被打开的时候，主页面应该停止滚动。在 CSS 中有如下的快捷实现方式：

> body{
> 
> overflow: hidden;
> 
> }

但上述代码会带来一点不良的副作用：

![](https://mmbiz.qpic.cn/mmbiz_gif/zPh0erYjkib1S4or5PlS9mBibR4uwkGagfVFryicxsTYj8JicKyfbYcOc3ibicGdGqqzZQysibsUmmFUlyKwz4S7mibiaXQ/640?wx_fmt=gif&tp=webp&wxfrom=5&wx_lazy=1)

（注意红色剪头）

在这个示例中，为了演示目的，我们在 Mac 系统中设置了强制显示滚动条，因而用户体验与 Windows 用户相似。

我们该如何解决这个问题呢？如果我们知道滚动条的宽度，每次当模态框出现时，可在主页面的右边设置一点边距。

由于不同的操作系统与浏览器对滚动条的宽度不一，因而获取它的宽度并不容易。在Mac 系统中，无论任何浏览器（滚动条）都是统一15px，然而 Windows 系统可会令开发者发狂：

![](https://mmbiz.qpic.cn/mmbiz_png/zPh0erYjkib1S4or5PlS9mBibR4uwkGagfELwBFv2dOYiciaiaDrbUcw1ia3CVGlTYexd6gH99OakkM9o56AF6Vnxmmw/640?wx_fmt=png&tp=webp&wxfrom=5&wx_lazy=1&wx_co=1)

（“百花齐放”的宽度）

注意，以上仅是 Windows 系统下基于当前最新版浏览器（测试所得）的结果。以前的（浏览器）版本（宽度）可能有所不同，也没人知道未来（滚动条的宽度）会如何变化。

不同于猜测（滚动条的宽度），你可以通过 JavaScript 计算它的宽度（译者注：实测以下代码仅能测出原始的宽度，通过 CSS 改变了滚动条宽度后，以下代码也无法测出实际宽度）：

> constouter = document.createElement('div');
> 
> constinner = document.createElement('div');
> 
> outer.style.overflow = 'scroll';
> 
> document.body.appendChild(outer);
> 
> outer.appendChild(inner);
> 
> constscrollbarWidth = outer.offsetWidth - inner.offsetWidth;
> 
> document.body.removeChild(outer);

尽管仅仅七行代码（就能测出滚动条的宽度），但有数行代码是操作 DOM 的。（为性能起见，）如非必要，尽量避免进行 DOM 操作。

解决这个问题的另一个方法是在模态框出现时仍保留滚动条，以下是基于这思路的纯 CSS 实现：

> html{
> 
> overflow-y: scroll;
> 
> }

尽管“模态框抖动”问题解决了，但整体的外观却被一个无法使用的滚动条影响了，这无疑是设计中的硬伤。

在我们看来，更好的解决方案是完全地隐藏滚动条。纯粹用 CSS 也是可以实现的。该方法（达到的效果）和 macOS 的表现并不是完全一致，（当用户）滚动时滚动条仍然是不可见的。滚动条总是处于不可见状态，然而页面是可被滚动的。对于Chrome，Safari 和 Opera 而言，可以使用以下的 CSS：

> .container::-webkit-scrollbar{
> 
> display: none;
> 
> }

IE 或 Edge 可用以下代码:

> .container{
> 
> -ms-overflow-style: none;
> 
> }

至于 Firefox，很不幸，没有任何办法隐藏滚动条。

正如你所见，并没有任何银弹。任何解决方案都有它的优点与缺点，应根据你项目的需要选择最合适的。

**外观争议**

需要承认的是，滚动条的样子在部分操作系统上并不好看。一些设计师喜欢完全掌控他们（所设计）应用的样式，任何一丝细节也不放过。在 GitHub 上有上百个库借助 JavaScript 取代系统滚动条的默认实现，以达到自定义的效果。

但如果你想根据现有的浏览器定制一个滚动条呢？（很遗憾，）并没有通用的 API，每个浏览器都有其独特的代码实现。

尽管5.5版本以后的 IE 浏览器允许你修改滚动条的样式，但它只允许你修改滚动条的颜色。以下是如何重新绘制（滚动条）拖动部分与箭头的代码：

> body{
> 
> scrollbar-face-color: blue;
> 
> }

但只改变颜色对提高用户体验而言帮助不大。据此，WebKit 的开发者在2009年提出了（修改滚动条）样式的方案。以下是使用 -webkit 前缀在支持相关样式的浏览器中模拟 macOS 滚动条样式的代码：

> ::-webkit-scrollbar{
> 
> width: 8px;
> 
> }
> 
> ::-webkit-scrollbar-thumb{
> 
> background-color: #c1c1c1;
> 
> border-radius: 4px;
> 
> }

Chrome、Safari、Opera 甚至于 UC 浏览器或者三星自带的桌面浏览器都支持（上述 CSS）。Edge 也有计划实现它们。但三年过去了，该计划仍在中等优先级中（而尚未被实现）。

当我们讨论滚动条的定制时，Mozilla 基金会基本上是无视了设计师的需求。（有开发者在）17年前就已经提出了一个希望修改滚动条样式的请求。而就在几个月前，Jeff Griffiths（Firefox 浏览器总监）终于为这个问题作出了回答：

“除非团队中有人对此有兴趣，否则我对此毫不关心。”

公平地说，从 W3C 的角度看来，尽管 WebKit 的实现得到广泛的支持，但它仍然不是标准。现有的为滚动条修改样式的草案，是基于 IE 的：仅能修改它的颜色。

伴随着请求如同 WebKit 一样支持滚动条样式修改 issue 的提交，争议仍在继续。如果你想影响 CSS 工作小组，是时候参与讨论了。也许这不是优先级最高的问题，但（如同 WebKit 一样修改滚动条样式）得到标准化后，能使很多前端工程师与设计师轻松很多。

**流畅的操作体验**

对于滚动而言，最常见的任务是登录页的导航（跳转）。通常，它是通过锚点链接来完成的。只需要知道元素的 id 即可：

> <ahref="#section"></a>

点击该链接会 跳 到（该锚点对应的）区块上，（然而） UX 设计师一般会坚持认为该过程应是平滑地运动的。GitHub 上有大量造好的轮子（帮你解决这个问题），然而它们或多或少都用到 JavaScript。（其实）只用一行代码也能实现同样的效果，最近DOM API 中的 Element.scrollIntoView() 可以通过传入配置对象来实现平滑滚动：

> elem.scrollIntoView({
> 
> behavior: 'smooth'
> 
> });

然而该属性兼容性较差且仍是通过脚本（来控制样式）。如有可能，应尽量少用额外的脚本。

幸运的是，有一个全新的 CSS 属性（仍在工作草案中），可以用简单的一行代码改变整个页面滚动的行为。

> html{
> 
> scroll-behavior: smooth;
> 
> }

结果如下:

![](https://mmbiz.qpic.cn/mmbiz_gif/zPh0erYjkib1S4or5PlS9mBibR4uwkGagfWOx5gubdeib6FTmh08zjnkyInQDMaOMiaaicFFhyPbYpNhVK6dOjbmQGw/640?wx_fmt=gif&tp=webp&wxfrom=5&wx_lazy=1)

（从一个区块跳到另一个）

![](https://mmbiz.qpic.cn/mmbiz_gif/zPh0erYjkib1S4or5PlS9mBibR4uwkGagfEozAicEbhcECu4T64I55snoEkt52a94fWs2aSiaG7ZlibiaRAbIYnpwxew/640?wx_fmt=gif&tp=webp&wxfrom=5&wx_lazy=1)

（平滑地滚动）

你可以在 codepen 上试验这个属性。在撰写本文时，scroll-behavior 仅在 Chrome、 Firefox 与 Opera 上被支持，但我们希望它能被广泛支持，因为使用 CSS （比使用 JavaScript）在解决页面滚动问题时优雅得多，并更符合“渐进增强”的模式。

**粘性 CSS**

另一个常见的需求是根据滚动方向动态地定住元素，即有名的“粘性（即 CSS 中的position: sticky）”效应。

![](https://mmbiz.qpic.cn/mmbiz_gif/zPh0erYjkib1S4or5PlS9mBibR4uwkGagf5LPnKbh9PNT5nlRtCWMeUsjQ1OoUtbOfSMDVXpgVjFlx7NRFV3XTtA/640?wx_fmt=gif&tp=webp&wxfrom=5&wx_lazy=1)
（一个粘性元素）

在以前的日子里，要实现一个“粘性”元素需要编写复杂的滚动处理函数去计算元素的大小。（然而）该函数较难处理元素在“黏住”与“不黏住”之间微小的延迟，（通常会）导致（元素）抖动的出现。通过 JavaScript 来实行（“粘性”元素）也有性能上的问题，特别是在（需要）调用 [Element.getBoundingClientRect() ]时(https://developer.mozilla.org/en-US/docs/Web/API/Element/getBoundingClientRect)。

不久之前，CSS 实现了 position: sticky 属性。只需通过指定（某方向上的）偏移量即可实现我们想要的效果。

> .element{
> 
> position: sticky;
> 
> top: 50px;
> 
> }

（编写上述代码后，）剩下的就交由浏览器实现即可。你可以在 codepen 上试验一下。撰写本文之时，position: sticky 在各式浏览器（包括移动端浏览器）上支持良好，所以如果你还在使用 JavaScript 去解决这个问题的话，是时候换成纯 CSS 的实现了。

**全面使用函数节流**

从浏览器的角度看来，滚动是一个事件，因此在 JavaScript 中是使用一个标准化的事件监听器 addEventListener 去处理它： ，

> window.addEventListener('scroll',() => {
> 
> constscrollTop = window.scrollY;
> 
> /* doSomething with scrollTop */
> 
> });

用户往往高频率地滚动（页面），但如果滚动事件触发太频繁的话，会导致性能上的问题，可以通过使用函数节流这一技巧去优化它。

> window.addEventListener('scroll',throttle(() => {
> 
> constscrollTop = window.scrollY;
> 
> /* doSomething with scrollTop */
> 
> }));

你需要定义一个节流函数包装原来的事件监听函数，（节流函数是）减少被包装函数的执行次数，只允许它在固定的时间间隔之内执行一次：

> functionthrottle(action,wait = 1000){
> 
> let time = Date.now();
> 
> returnfunction(){
> 
> if((time + wait - Date.now()) < 0){
> 
> action();
> 
> time = Date.now();
> 
> }
> 
> }
> 
> }

为了使（节流后的）滚动更平滑，你可以通过使用 window.requestAnimationFrame() 来实现函数节流：

> functionthrottle(action){
> 
> let isRunning = false;
> 
> returnfunction(){
> 
> if(isRunning)return;
> 
> isRunning = true;
> 
> window.requestAnimationFrame(() => {
> 
> action();
> 
> isRunning = false;
> 
> });
> 
> }
> 
> }

当然，你可以通过现有的开源轮子来实现，就像 Lodash 一样。你可以访问 codepen 来看看上述解决方案与 Lodash 中的 _.throttle 之间的区别。

使用哪个（开源库）并不重要，重要的是在需要的时候，记得优化你（页面中的）滚动处理函数。

**在视窗中显示**

当你需要实现图片懒加载或者无限滚动时，需要确定元素是否出现在视窗中。这可以在事件监听器中处理，最常见的解决方案是使用 lement.getBoundingClientRect() ：

> window.addEventListener('scroll',() => {
> 
> constrect = elem.getBoundingClientRect();
> 
> constinViewport = rect.bottom > 0 && rect.right > 0 &&
> 
> rect.left < window.innerWidth &&
> 
> rect.top < window.innerHeight;
> 
> });

上述代码的问题在于每次调用 getBoundingClientRect 时都会触发回流，严重地影响了性能。在事件处理函数中调用（ getBoundingClientRect ）尤为糟糕，就算使用了函数节流（的技巧）也可能对性能没多大帮助。 （回流是指浏览器为局部或整体地重绘某个元素，需要重新计算该元素在文档中的位置与形状。）

在2016年后，可以通过使用 Intersection Observer 这一 API 来解决问题。它允许你追踪目标元素与其祖先元素或视窗的交叉状态。此外，尽管只有一部分元素出现在视窗中，哪怕只有一像素，也可以选择触发回调函数：

> constobserver = newIntersectionObserver(callback,options);
> 
> observer.observe(element);

此 API 被广泛地支持，但仍有一些浏览器需要 polyfill。尽管如此，它仍是目前最好的解决方案。

**滚动边界问题**

如果你的弹框或下拉列表是可滚动的，那你务必要了解连锁滚动相关的问题：当用户滚动到（弹框或下拉列表）末尾（后再继续滚动时），整个页面都会开始滚动。

![](https://mmbiz.qpic.cn/mmbiz_gif/zPh0erYjkib1S4or5PlS9mBibR4uwkGagffibNW1A69DE9caPYibwW3UcjXNLz1KWPMNBxRvgiakklYsRfmKwicN0lXw/640?wx_fmt=gif&tp=webp&wxfrom=5&wx_lazy=1)

（连锁滚动的表现）

当滚动元素到达底部时，你可以通过（改变）页面的 overflow 属性或在滚动元素的滚动事件处理函数中取消默认行为来解决这问题。

如果你选择使用 JavaScript （来处理），请记住要处理的不是“scroll（事件）”，而是每当用户使用鼠标滚轮或触摸板时触发的“wheel（事件）”：

> functionhandleOverscroll(event){
> 
> constdelta = -event.deltaY;
> 
> if(delta< 0 && elem.scrollHeight - elem.scrollTop){
> 
> elem.scrollTop = elem.scrollHeight;
> 
> event.preventDefault();
> 
> returnfalse;
> 
> }
> 
> if(delta > elem.scrollTop){
> 
> elem.scrollTop = 0;
> 
> event.preventDefault();
> 
> returnfalse;
> 
> }
> 
> returntrue;
> 
> }

不幸的是，这个解决方案不太可靠。同时可能对（页面）性能产生负面影响。

过度滚动对移动端的影响尤为严重。Loren Brichter 在 iOS 的 Tweetie 应用上创造了一个“下拉刷新”的新手势，这在 UX 社区中引起了轰动：包括 Twitter 与 Facebook 在内的各大应用纷纷采用了（相同的手势）。

当这个特性出现在安卓端的 Chrome 浏览器中时，问题出现了：它会刷新整个页面而不是加载更多的内容，成为开发者在他们的应用中实现“下拉刷新”时的麻烦。

CSS 通过 overscroll-behavior 这个新属性解决问题。它通过控制元素滚动到尽头时的行为来解决下拉刷新与连锁滚动所带来的问题，（它的属性值中）也包含针对不同平台特殊值：安卓的 glow 与 苹果系统中的 rubber band。

现在，上面 GIF 中的问题，在 Chrome、Opera 或 Firefox 中可以通过以下一行代码来解决：

> .element{
> 
> overscroll-behavior: contain;
> 
> }

公平地说，IE 与 Edge 实现了（它独有的） -ms-scroll-chaining 属性来控制连锁滚动，但它并不能处理所有的情况。幸运的是，根据这消息，微软的浏览器已经准备实现 overscroll-behavior 这一属性了。

**触屏之后**

触屏设备上的滚动（体验）是一个很大的话题，深入讨论需要另开一篇文章。然而，由于很多开发者忽略了这方面的内容，这里需要提及一下。

（滚动手势无处不在，令人沉迷，以至于想出了如此疯狂的主意去解决“滚动上瘾”的问题。）

周围的人在智能手机屏幕上上下移动他们的手指的频率是多少呢？经常这样对吧，当你阅读本文时，你很可能就在这么做。

当你的手指在屏幕上移动时，你期待的是：页面内容平滑且流畅地移动。

苹果公司开创了“惯性”滚动并拥有它的专利 。它讯速地成为了用户交互的标准并且我们对此已习以为常。

但你也许已经注意到了，尽管移动端系统会为你实现页面上的惯性滚动，但当页面内某个元素发生滚动时，即使用户同样期待惯性滚动，但它并不会出现，这令人沮丧。

这里有一个 CSS 的解决方案，但看起来更像是个 hack：

> .element{
> 
> -webkit-overflow-scrolling: touch;
> 
> }

为什么这是个 hack 呢？首先，它只能在支持（webkit）前缀的浏览器上才能工作。其次，它只适用于触屏设备。最后，如果浏览器不支持的话，你就这样置之不理吗？但无论如何，这总归是一个解决方案，你可以试着使用它。

在触屏设备上，另一个需要考虑的问题是开发者如何处理 touchstart 与 touchmove 事件触发时可能存在的性能问题，它对用户滚动体验的影响非常大。这里详细描述了整个问题。简单来说，现代的浏览器虽然知道如何使得滚动变得平滑，但为确认（滚动）事件处理函数中是否执行了 Event.preventDefault() 以取消默认行为，有时仍可能需要花费500毫秒来等待事件处理函数执行完毕。

即使是一个空的事件监听器，从不取消任何行为，鉴于浏览器仍会期待 preventDefault 的调用，也会对性能造成负面影响。

为了准确地告诉浏览器不必担心（事件处理函数中）取消了默认行为，在 WHATWG 的 DOM 标准中存在着一个不太显眼的特性（能解决这问题）。（它就是）Passive event listeners，浏览器对它的支持还是不错的。事件监听函数新接受一个可选的对象作为参数，告诉浏览器当事件触发时，事件处理函数永远不会取消默认行为。（当然，添加此参数后，）在事件处理函数中调用 preventDefault 将不再产生效果。

> element.addEventListener('touchstart',e => {
> 
> /* doSomething */
> 
> },{passive: true});

针对不支持该参数的浏览器，这里也有一个 polyfill 。这视频清晰地展示了此改进带来的影响。

**旧技术运行良好，为何还要改动？**

在现代互联网中，过渡地依赖 JavaScript 在各浏览器上实现相同的交互效果不再是合理的，“跨浏览器兼容性”已经成为过去式，更多的 CSS 属性与 DOM API 方法正逐步被各大浏览器所支持。

在我们看来，当你的项目中，有特别酷炫的滚动效果时，渐进增强是最好的做法。

你应该提供（给用户）所有（你能提供的）基础用户体验，并逐步在更先进的浏览器上提供更好的体验。

必要时使用 polyfill，它们不会产生（不必要的）依赖，一旦（某个 polyfill 所支持的属性）得到广泛地支持，你就可以轻松地将它删掉。

六个月之前，在本文尚未成文之时，之前我们描述的属性只被少量的浏览器所支持。而到了本文发表之时，这些属性已被广泛地支持。

也许到了现在，当你上下翻阅本文之时，（之前不支持某些属性的）浏览器已经支持了该属性，这使得你编程更容易，并使你的应用打包出来体积更小。

感谢阅读至此！查阅浏览器的更新日志，积极参与讨论，有助于 web 标准驶向正确的方向。祝大家一帆风顺，顺利滑（滚）向未来！