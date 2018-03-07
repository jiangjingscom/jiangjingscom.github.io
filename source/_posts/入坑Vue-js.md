---
title: Vue.js使用
date: 2017-05-26 09:41:27
categories: Font-end
tags: JavaScript
---
   又有一个多月没写博客了，这篇记录一下最近一个月忙的事情——将使用AngularJS(v1.5.4)框架写的后台换成Vue.js(v2.2.6)啦~巨开心！
至于我为什么换,大概是因为每次版本迭代不想按原来的方式改写一遍？时间还算充足？浏览器支持很宽松？想体验ES6写法？听说Vue很棒棒哇？想集齐四大法宝(JQuery/AngularJS/Vue.js/React)......主要原因应该是后台就我一个人在做，想怎么做就怎么做吧~哈哈= =
Vue框架使用非常方便，一如它所宣称的轻量高效。而且它的[官方文档](https://cn.vuejs.org/v2/guide/)超级清晰，[api](https://cn.vuejs.org/v2/api/)对于本菜来说也够用。之前用过几个月的Angular,文档到现在还没有全部看完（嗯，可能我比较懒...但它的东西真的多的不要不要的），但是Vue,它的文档我已经刷了好几遍（闲来无事刷文档）= =对于我这种准备学习一门语言或框架之前往往需要先完完整整过一两遍比较全的书或文档，才能放心开始学习的人来说，简直不要太棒！嗯，少说废话，进入正题。
首先搭一个框架，做好准备工作。用vue-cli脚手架工具(npm install vue-cli -g)，初始化项目(vue init webpack-simple admin)很快得到admin项目下的完整目录：

<img src="../../../../assets/img/5-31-1.png"   align=center />

<!--more-->
详细的网上有教程，其中build和config里面放的是一些配置文件(主要是webpack的，用于打包),dist中放的是开发好的目录，如果要直接放在服务器中访问，需要简单修改一下config文件中的index.js,static放静态资源，src就是我们的开发目录啦。
除了vue-cli帮我们安装的一些npm包，由于项目需要，另外引入一些工具

``` json
  "dependencies": {
    // 解决http请求
    "axios": "^0.16.1",
    // 解决Babel对于一些api(如Promise等)不转码的问题
    "babel-polyfill": "^6.23.0",
    // 强大的时间操作工具
    "moment": "^2.18.1",
    // 简单的tooltip
    "v-tooltip": "^2.0.0-beta.4",
    "vue": "^2.2.6",
    // vue 官方路由
    "vue-router": "^2.3.1",
    // vue官方状态管理模式
    "vuex": "^2.3.1"
  },
```

其中，[vuex](https://vuex.vuejs.org/zh-cn/getting-started.html)和[vue-router](https://router.vuejs.org/zh-cn/)都有官方文档，十分详细。引入axios是因为vue不像angular，angular提供的$http服务同服务端通信，相比vue-resource,官方推荐强大的axios！用上babel-polyfill工具，因为想使用一些ES6新的api，真想把它们统统都夸一遍 = =
看一下项目目录：

<img src="../../../../assets/img/5-31-2.png"   align=center />

component: 一些组件(toast,tree,modal,avatar),由于不想引入大而全的库(很多东西不需要)，模式组件源于自己写写改改。
directive: 一些指令(v-focus)
filter: 一些过滤器(时间、文件大小，操作记录等等)，另外，可直接在组件内部使用computed,有时候它更好用。
libs: 一些工具，引用库
router: 配置vue路由
service: 所有http请求，按照原来在angular框架时师傅教的，将所有api独立出来，感觉好写好改好查找。
store: vue的状态管理，我主要放了个人信息和组织结构树信息，方便管理数据
views: 所有路由页(因为在做单页应用，在只用component还是用部分router两种模式比较了一下，发现都差不多，为了用上全家桶，用上router)
展开的就不放了，因为文件特别多 = =
一个简单的component样式：

<img src="../../../../assets/img/5-31-3.png"   align=center />

<img src="../../../../assets/img/5-31-4.png"   align=center />


未完，待续......

----2018-3-1 更新---- 
    待续？用完三个框架，感觉Vue最方便好用，简单易入门，解决了数据绑定、组件开发、页面路由等痛点，配套Vuex解决数据管理，差不多了，对于我之前写的小项目来说，好用，没毛病...
    有机会还会再用，好评。\(^o^)/~