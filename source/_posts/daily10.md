---
title: 2019年React学习路线图
date: 2018-12-20 19:05:40
tags: react
categories: 分享
---
> 作者｜javinpaul
> 译者｜无明
> 转发自 | 前端之巅公众号

之前我们已经介绍了 [2019 年 Vue 学习路线图](https://mp.weixin.qq.com/s?__biz=MzUxMzcxMzE5Ng==&mid=2247490087&idx=1&sn=fb16b7826416244642cdab69a52848c0&chksm=f951af64ce262672982f1896976f594589925a0b2730801715247ae40d7ee06c2960d6b6a338&token=1582750074&lang=zh_CN&scene=21#wechat_redirect)，而 React 作为当前应用最广泛的前端框架，在 Facebook 的支持下，近年来实现了飞越式的发展，我们将在下文中介绍 2019 年 React 学习路线图，希望给想学 React 的开发者一些借鉴。

下图是2018 年的 React 路线图，它非常全面，2018 年剩下的时间可能不够你学会所有这些，但不要担心，所有的技术在 2019 年仍然有效。

![](https://mmbiz.qpic.cn/mmbiz_png/XIibZ0YbvibkUhfSXs7kotWIegAeUWWHMDpOs46GfwR0YnIHCauC8Kt69U6gexHpKQvqZpFyhcUABEJYYCnTFwbA/640?wx_fmt=png&tp=webp&wxfrom=5&wx_lazy=1&wx_co=1)
图片来源：

https://github.com/adam-golab/react-developer-roadmap/blob/master/roadmap.png

### 1、基础知识

不管你要学习哪个 Web 开发框架或库，都必须掌握基础知识，如 HTML、CSS 和 JavaScript，这三个是 Web 开发的三大支柱。

**HTML**

HTML 是 Web 开发人员最重要的技能之一，因为它为网页提供了基本结构。

**CSS**

CSS 用于设置网页样式，让网页看起来更好看。

**JavaScript**

JavaScript 让网页具备交互性。React 是基于 JavaScript 的，因此在学习 React 之前，你应该先了解 JavaScript。

### 2、通用的开发技能

无论你是前端开发人员还是后端开发人员，甚至是全栈工程师，都必须了解一些能够让你在编程世界中生存下来的通用开发技能。

**学习 GIT**

你必须在 2018 年完全了解 Git。尝试在 GitHub 上创建一些存储库，与其他人共享你的代码，并学习如何在你喜欢的 IDE 中克隆 Github 上的代码。

**了解 HTTP(S) 协议**

如果你想成为一名 Web 开发人员，那么了解 HTTP 绝对是有必要的。

我不是要你去阅读 HTTP(S) 规范，但你至少应该熟悉常见的 HTTP 请求方法，如 GET、POST、PUT、PATCH、DELETE、OPTIONS 以及 HTTP/HTTPS 的工作原理。

**学习终端**

虽然前端开发人员学习 Linux 或终端并不是强制性的，但我强烈建议你熟悉以下终端，了解如何配置你的 shell（bash、zsh、csh）等。

**算法和数据结构**

好吧，这又是一个通用编程技能，成为 React 开发者不一定需要了解这些，但要成为真正的程序员，这是必备技能。

**学习设计模式**

就像算法和数据结构一样，成为 React 开发者并不一定要学习设计模式，但学好设计模式会让你变得更好。了解设计模式将帮你找到能够经受住时间考验的解决方案。

### 3、学习 React

你必须学好 React 才能成为一名 React 开发者。学习 React 最好的资源是它的官方网站，但作为初学者，它对你来说可能有点难。

**学习构建工具**

如果你想成为一名专业的 React 开发者，那么你应该花一些时间熟悉一下你将作为 Web 开发者需要使用的工具，比如构建工具、单元测试工具、调试工具等。

以下是路线图中列出的构建工具：

包管理器：

-   npm
    
-   yarn
    
-   pnpm
    
-   任务执行器
    
-   npm 脚本
    
-   gulp
    
-   WebPack
    
-   Rollup
    
-   Parcel
    

顺便说一句，并非要学习所有这些工具，对于初学者来说，学习 npm 和 Webpack 应该足够了。在你对 Web 开发和 React 生态系统有了更多的了解后，你就可以学习其他工具。

**样式**

如果你的目标是成为 React 开发者，了解一些样式相关的知识只会有益无害。路线图中提到了很多 CSS 相关的东西，比如 CSS 预处理器、CSS 框架、CSS 架构和 JS 中的 CSS。

我建议你至少学习一下 Bootstrap，这是你经常会用到的 CSS 框架。

如果你想进一步学习 bootstrap，也可以学习 Materialise 或 Material UI。

**状态管理**

这是 React 开发者应该关注的另一个重要领域。路线图中提到了以下一些需要掌握的概念和框架：

-   组件 State/ContextAPI
    
-   Redux
    
-   异步操作（副作用）
    
-   Redux Thunk
    
-   Redux Better Promise
    
-   Redux Saga
    
-   Redux Observable
    
-   Helpers
    
-   Rematch
    
-   Reselect
    
-   Data persistence
    
-   Redux Persist
    
-   Redux Phoenix
    
-   Redux Form
    
-   MobX
    

如果东西太多，我建议你只关注 Redux。

**Type Checker**

由于 JavaScript 不是一种强类型语言，因此编译器不会捕获那些与类型相关的错误。

随着应用程序的增长，你可以通过类型检查捕获大量错误，尤其是如果你可以使用 Flow 或 TypeScript 等 JavaScript 扩展对整个应用程序进行类型检查。

React 也提供了一些内置的类型检查功能，可以用它们帮你尽早发现 bug。

由于 Angular 也使用了 TypeScript，我认为可以同时学习 JavaScript 和 TypeScript。

**Form Helper**

除了 Type Checker 之外，还可以学习像 Redux Form 这样的 Form Helper，它提供了在 Redux 中管理表单状态的最佳方法。除了 Redux Form 之外，还有 Formik、Formsy 和 Final。

**路由**

组件是 React 声明性编程模型的核心，而路由组件是应用程序的重要组成部分。

React Router 提供了一组导航组件，这些组件可以通过声明的方式与你的应用程序组合在一起。

除了 React Router 之外，你还可以看看 Router 5 和 Redux-First Router。

**API 客户端**

在今天的世界中，你很少会构建独立的 GUI，相反，你将有更多机会使用 REST 和 GraphQL 等 API 构建与其他应用程序发生交互的东西。

值得庆幸的是，React 开发者可以使用很多 API 客户端：

REST

-   Fetch
    
-   SuperAgent
    
-   axios
    

GraphQL

-   Apollo
    
-   Relay
    
-   urql
    

Apollo 客户端是我的最爱，它提供了一种使用 GraphQL 构建客户端应用程序的简便方法。Apollo 可以帮你快速构建使用 GraphQL 获取数据的 UI，并可以与任意 JavaScript 前端一起使用。

**辅助库**

这些库可以让你的工作变得更轻松。React 开发人员可以使用很多辅助库，如下所示：

-   Lodash
    
-   Moment
    
-   classnames
    
-   Numeral
    
-   RxJS
    
-   Ramda
    

这些不一定都要学，路线图中的 Lodash、Moment 和 Classnames 是用黄色标注的，所以应该先从它们开始学习。

**测试**

测试是 React 开发者的一项重要技能，但经常被忽视，如果你想在竞争中保持领先，就要学习一些用于测试的库。这些库可用于单元测试、集成测试和端到端测试。

以下是路线图中提到的库：

**单元测试**

-   Jest
    
-   Enzyme
    
-   Sinon
    
-   Mocha
    
-   Chai
    
-   AVA
    
-   Tape
    

**端到端测试**

-   Selenium, Webdriver
    
-   Cypress
    
-   Puppeteer
    
-   Cucumber.js
    
-   Nightwatch.js
    

**集成测试**

-   Karma
    

你可以学习你想学习的库，但建议一定要学习 Jest 和 Enzyme。

**国际化**

这是前端开发的另一个重要主题。你可能需要支持日本、中国、西班牙和其他欧洲国家的本地 GUI 版本。

路线图中建议你学习以下技术，它们都很好理解：

-   React Intl
    
-   React i18next
    

这两个库都提供了 React 组件和 API 来格式化日期、数字和字符串，包括复数和处理翻译。

**服务器端渲染**

你可能会想，服务器端渲染和客户端渲染之间有什么区别。在使用客户端渲染时，你的浏览器会下载一个最小的 HTML 页面，然后通过 JavaScript 并将内容填充到页面中。

在使用服务器端渲染时，React 组件是在服务器上进行渲染的，将输出的 HTML 内容传到客户端或浏览器。

路线图推荐了以下的服务器端渲染：

-   Next.js
    
-   After.js
    
-   Rogue
    

不过我建议学习 Next.js 应该足够了。

**静态站点生成器**

Gatsby.js 是一个现代静态站点生成器。你可以使用 Gatsby 创建个性化的登录网站体验。它将你的数据与 JavaScript 相结合，并创建格式良好的 HTML 内容。

**后端框架集成**

React on Rails 将 Rails 与 Facebook 的 React 前端框架（服务器渲染）集成在一起。它提供了服务器渲染，通常用于 SEO 爬虫索引和 UX。

### 4、移动端

React Native 正迅速成为使用 JavaScript 开发具有原生外观的移动应用程序的标准方法。

路线图中建议你学习以下库：

-   React Native 

-   Cordova/PhoneGap

-   Flutter-----------------本人添加

但我认为只要学习 React Native 就足够了。
这里补充一句我个人想法，现在应该要关注一下Flutter的发展了-----------------本人添加

### 5、桌面端

还有一些基于 React 的框架可用于构建像 React Native Windows 这样的桌面 GUI，让你可以使用 React 构建原生 UWP 和 WPF 应用程序。

路线图建议使用以下几个库：

-   Proton Native
    
-   Electron
    
-   React Native Windows
    

它们都是进阶的内容，如果你已经掌握了 React，可以看一下它们。

### 6、虚拟现实

如果你对构建基于虚拟现实的应用程序感兴趣，还可以了解以下像 React 360 这样的框架，让你可以通过 React 开发 VR 体验。如果你对这个领域感兴趣，可以进一步了解 React 360。

英文原文：

https://hackernoon.com/the-2018-react-js-roadmap-4d0a43814c02