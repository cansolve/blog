---
title: 用Hexo + github搭建自己的博客
date: 2018-4-2 19:25:46
tags: 建站
categories: 个人
---

# **前言**
这是我第一次写这种类型的博客，也不叫什么博客，纯粹个人一些踩坑过程罢了，原先弄的前后端分离的开发流程想想太复杂，一个博客页面没啥必要，主要还是记录自己的一些心得，内容比较重要。话不多说，开搞！！！

## 基于node和git
安装[node.js](http://nodejs.cn/)和[git](https://git-scm.com/downloads)
这个就不多说了，贴个地址

## **快速开始**
1、找个文件夹下打开终端,输入

    hexo i blogName //blog是项目名
    cd blogName //切换到站点根目录
    hexo g //generetor的缩写
    hexo s //server的缩写

2 打开浏览器输入localhost:4000查看：
    ![local](http://img.cansolve.cn/20161115143629057.jpg "localhost:4000")

看到这个样子就说明成功了，这个就是hexo默认的博客主题。现在你已经可以在这个主题下写博客了。
你还可以选择博客的主题[theme](https://hexo.io/themes/)

## **选择主题**
我选的是Claudia

1 . 在站点根目录输入

    git clone https://github.com/Haojen/hexo-theme-Claudia.git

2 . 完成后，打开根目录下的 _config.yml 文件， 找到 theme 字段，把landscape改为 Claudia
    ![theme](http://img.cansolve.cn/2018112110001.png "theme")

3 . 在终端输入

    hexo clean  //清除缓存
    hexo g  //重新生成代码
    hexo s  //部署到本地

    //然后打开浏览器访问 localhost:4000 查看效果

![theme](http://img.cansolve.cn/201812110002.png "theme")
这时候主题已经换了，主题里面的修改项自行查阅一下，很多都有注释

## **上传到github**
没有github账号的，自行注册一个【很少人没有吧】

![theme](http://img.cansolve.cn/201812110003.png "theme")
完了继续下一步选择一个主题

![theme](http://img.cansolve.cn/201812110004.jpg "theme")
结束访问 xxxxx.github.io  会看到上面一样的页面

修改文件
![theme](http://img.cansolve.cn/201812110005.png "theme")

    注意！！！冒号的后面一定一定一定要有一个空格！！

开始部署

    npm install hexo-deployer-git --save
    //先装个插件
    hexo d  //  部署的命令

网上有的教程说需要账号密码，我这边没遇到，所以就不方便截图了

## **发布第一篇博客**
根目录下输入 ：

    hexo new "postName"
    //hexo n 也可以 
    //你自己的博客名称，名为postName.md的文件会建在目
    //录/blog/source/_posts下。

文章编辑完成后，终端在根目录文件夹下，执行如下命令来发布:

    hexo g //生成静态页面，类似于打个包
    hexo d //发布

这样就可以发布咯