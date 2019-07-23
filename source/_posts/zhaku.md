---
title: 写了个前端组件库 - zhaKU
date: 2019-04-20 12:23:45
tags: vue.js
category: 前端
---
![](https://images.unsplash.com/photo-1561736778-92e52a7769ef?ixlib=rb-1.2.1&ixid=eyJhcHBfaWQiOjEyMDd9&auto=format&fit=crop&w=1200&q=10)
<!-- more -->


## zhaKU

[文档地址](https://zhayes.github.io/zhaKU)

基于vue 2.x开发的UI组件库，轻量，简洁。

图标库基于[手机淘宝图标库](https://www.iconfont.cn/collections/detail?spm=a313x.7781069.1998910419.d9df05512&cid=33)

### 安装： 
```javascript
    npm install --save zhaku
```

### 引入：
```javascript
    import zhaku from 'zhaku';
    const {Button, Input} from 'zhaku';
```

### 全局引入：
```javascript
    import Vue from 'vue';
    import zhaku from 'zhaku';
    
    Vue.use(zhaku);
```

全局引入后，在模版里使用，只需用小写字母`z`加组件名，并用`-`连接。

例如：```<z-button>这个是按钮</z-button>```。

后续将完善更多的组件。
