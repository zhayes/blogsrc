---
title: 手写js常用方法
date: 2018-12-11 16:01:24
category: 前端
tags: javascript
---

![](https://images.unsplash.com/photo-1500100586562-f75ff6540087?ixlib=rb-1.2.1&auto=format&fit=crop&w=1200&q=10)
<!--more-->

不管用什么工具写，反正得用到手，所以这个标题似乎并没有什么不妥。
下面进入正题。

Promise是个用于异步执行的方法类，他的显著特点是支持链式调用，避免了之前异步执行嵌套回调那种写法的弊病，毕竟代码是给人看的，回调地狱的写法看起来就费劲。

```javascript

    function MyPromise(callback) {
        var self = this;
        self.status = 'pending';
        self.data = null;
        self.onResolvedCbs = [];
        self.onRejectedCbs = [];

        try {
            callback(resolve, reject);
        } catch (err) {
            reject(err);
        }

        function resolve(data) {
            self.status = "fullfiled";
            self.data = data;
            self.onResolvedCbs.map((item) => {
                item(self.data);
            })
        }


        function reject(error) {
            self.status = 'rejected';
            self.data = error;
            self.onRejectedCbs.map((item)=>{
                item(self.data);
            })
        }
    }


    MyPromise.prototype.then = function (onResolve, onReject) {
        const self = this;

        const p = new MyPromise((resolve, reject)=>{
            
            if (self.status === 'fulfilled') {
                onResolve(self.data);
            }

            if (self.status === 'rejected') {
                onResolve(self.data);
            }
        
            if (self.status === 'pending') {
                self.onResolvedCallback.push(onResolve);
                self.onRejectedCallback.push(onReject);
            }
        
        });

        return p;
    };


    var p = new MyPromise((resolve, reject)=>{
        setTimeout(()=>{
            console.log('hi');
            resolve('success');
        },10);
    })


    p.then((data)=>{
        console.log(data);
    })

```

关于then链式调用的详细实现后面再说吧

---

实现一个 `curry function`


```javascript

    function curry(fn){
        var len = fun.length;

        return function exect(...args){
            if(args.length>=len){
                return fn.apply(null, args);
            }

            return function(...args2){
                return exect.apply(null, args.concat(args2));
            }
        }
    }

```
---

实现一个compose函数

```javascript

    function compose(...fn){
        return fn.reduce(function(preFn, nextFn){
            return function(...args){
                return preFn(nextFn(...args))
            }
        })
    }

```
---

模拟`redux`的`combineReducers`
网上`combineReducers`的用处说是将多个`reducer`进行合并。我开始看到这里一想`reducer`不就是那些`switch case`之类的判断吗。那他所谓的合并莫不是叠加`case`？我当时就想那不干脆用字符拼接嘛，然后用eval执行？哈哈哈哈～我这直观的脑路啊！

`combineReducers(state)`里面的参数实际就是你全局的`state`的数据格式，他可以让你一个key对应一个reduce方法。他们所谓的好吧实际就是遍历这个state对象的所有的key，并且执行对应的reduce方法，以改变其value。那这就很简单了嘛！

```javascript
    function combineReducers(reduceObj={}){
        var newState = {};
        return function(state, action){
            var keys = Object.keys(reduceObj);
            var hashChange = false;
            
            for(var i=0; i<keys.length; i++){
                var key = keys[i];
                var value = typeof reduceObj[key] === 'function' ? reduceObj[key](state[key], action) : reduceObj[key];

                hashChange = hashChange || value!=state[key];

                newState[key] = value;
            }

            return hashChange ? newState : state;
        }
    }
```
---

实现数组扁平化

```javascript

    function flatten(array){
        return array.reduce(function(pre, next){
            return pre.concat(Array.isArray(next) ? flatten(next) : next )
        }, [])
    }

```
---

js的`instance of`方法主要是判断一个对象是不是某个指定构造函数的实例，其判断的来源于该对象的`__proto__`属性是不是指定构造函数的原型，并且这种判断是可以顺着原型链往上追溯的，所以方法的实现核心就是判断构造函数的`prototype`跟实例的`__proto__`是不是一致。

```javascript
    function instanceOf(instance, constructor){
        var pro = instance.__proto__;
        while(true){
            if(pro===null){
                return false;
            }

            if(pro===constructor.prototype){
                return true;
            }

            pro = pro.__proto__;
        }
    }
```