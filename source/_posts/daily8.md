---
title: es6中的promise解读
date: 2018-11-23 19:45:40
tags: es6
categories: 分享
---
> 作者：离秋
> 链接：https://juejin.im/post/5bed21156fb9a04a0c2e025d

> 简单的说它是一个异步流程的控制手段。是一个代表了异步操作最终完成或者失败的对象。

### promise的优点

-   promise解决了回调地狱的问题
    
-   promise可以支持多个并发的请求
    
-   promise的错误传播机制可以统一的处理错误信息
    

### 回调地狱问题

> 在传统的ajax调用过程中，下面以jquery.ajax为例。如果需求中要多次进行ajax交互，并且上一次的返回结果还要被下一次的ajax使用，代码基本上会变成：

```
$.ajax({
   type: "POST",
   url: "some.php",
   data: "name=John&location=Boston",
   success: function(msg){
       $.ajax({
           type: "POST",
           url: "some.php",
           data: msg,
           success: function(msg2){
             ...//多次调用
           }
        });
   }
});
```

> 现在还只是两次调用关系，如果是多次调用将会引发下面的问题

1.  多次调用不利于代码的管理于维护
    
2.  发生错误时不能及时准备的定位错误位置
    
3.  只要又一次不成功就不能进行下面的逻辑，不方便进行错误处理。
    

> promise的链式调用就很好的解决了这个问题

### Promise的三种状态

-   Pending Promise对象实例创建时候的初始状态
    
-   resolve 可以理解为成功的状态
    
-   Reject 可以理解为失败的状态
    

> promise 中的状态只能是从等待状态转换到成功状态或者失败状态，且状态转变之后不可逆。

![](https://user-gold-cdn.xitu.io/2018/11/15/167164a42f989193?imageView2/0/w/1280/h/960/format/webp/ignore-error/1)![](https://user-gold-cdn.xitu.io/2018/11/15/167164a42f989193?imageView2/0/w/1280/h/960/format/webp/ignore-error/1)

### 一个简单的promise

```
let  p  =  new  Promise((resolve, reject) => {
    console.log(1)
});
console.log(2)
//1 2
```

> promise里面只接受一个参数，叫做执行器函数，这个函数会同步执行。也就是说上面代码中的箭头函数被同步执行，得到的结果也就是1和2

### promise中的then

> 每一个promise的实例上都有一个then方法，这个方法上有两个参数，一个是成功的回调，一个是失败的回调。而取决于成功或者失败的是promise的执行器函数中执行的是成功还是失败。

```
let  p  =  new  Promise((resolve, reject) => {
    console.log(1);
    resolve();//调用resolve会走到then的成功回调
    //reject();//调用resolve会走到then的失败回调
});
p.then(
    () => {
        console.log('成功')
    }
    , () => {
        console.log('失败')
    })
    //1 成功
```

> 如果既不调用resolve也不调用reject，promise则一直处于等待状态，也就不会走到then方法。

```
let  p  =  new  Promise((resolve, reject) => {
    console.log(1);
});
p.then(
    () => {
        console.log('成功')
    }
    , () => {
        console.log('失败')
    })
    //1 
```

> 如果你既调用resolve也调用reject，那么谁在前面执行就走谁的对应回调函数

```
let  p  =  new  Promise((resolve, reject) => {
    console.log(1);
    resolve()//先调用成功
    reject()
});
p.then(
    () => {
        console.log('成功')
    }
    , () => {
        console.log('失败')
    })
    //1 成功
```

```
let  p  =  new  Promise((resolve, reject) => {
    console.log(1);
    reject()//先调用失败
    resolve()
});
p.then(
    () => {
        console.log('成功')
    }
    , () => {
        console.log('失败')
    })
    //1 失败
```

> 如果代码出错则会直接走reject的回调

```
let  p  =  new  Promise((resolve, reject) => {
    console.log(1);
    throw  new  Error('出错了~')
});
p.then(
    () => {
        console.log('成功')
    }
    , () => {
        console.log('失败')
    })
    //1 失败
```

> 一个promise的实例可以then多次

```
let  p  =  new  Promise((resolve, reject) => {
    resolve('成功了');
});
p.then((data) => {
    console.log(data)//成功了
});
p.then((data) => {
    console.log(data)//成功了
})
```

### 利用promise解决回调地狱

> 能够规避异步操作中回调地狱的问题，其本质取决于promise的链式调用。 假设需求如下，a.txt文件的内容为b.txt,b.txt文件的内容是一段描述文字，现在要求用a.txt的得到最终的描述文字，代码如下：

```
let  fs  =  require('fs');
//首先将异步方法封装在一个promise中，异步结果成功调用resolve方法，失败调用reject方法。
function  read(url) {
    return  new  Promise((resolve, reject) => {
        fs.readFile(url, 'utf8', function (err, data) {
            if (err) reject();
            resolve(data);
        })
    })
}
//因为read方法返回的是一个promise，所以可以使用promise的then方法
read('a.txt').then((data) => {
//第一次异步成功后拿到结果继续返回一个promise可以实现链式调用。
    console.log(data);//b.txt
return  read(data);
}, (err) => { }).then((data) => {
//最后两次的结果分别对应两次异步的返回内容
    console.log(data)//描述文字
}, (err) => { })
```

> 总结：如果一个promise的then方法中还返回另一个promise，那么这个promise的成功状态会走到外层promise的下一次then方法的成功，如果失败，返回外层promise下一次then的失败。

### promise的链式调用

-   如果then中返回的是一个普通值，就会走到下一次then的成功回调。
    

```
read().then((data) => {
return  111
}, (err) => { }).then((data) => {
console.log(data)//111
}, (err) => { })
```

-   如果then中返回的是一个错误，就会走到下一次then的失败回调。
    

```
read().then((data) => {
throw  new  Error('出错了~')
}, (err) => { }).then((data) => {
console.log(data)
}, (err) => {
console.log(err)//出错了~
})
```

-   如果then中什么也不返回，就会走到下一次then的成功回调，得到的值为undefined。
    

```
read().then((data) => {
cons.log(111)
}, (err) => { }).then((data) => {
console.log(data)//undefined
}, (err) => {
})
```

-   如果想统一处理错误内容，可以使用catch。
    

```
read().then((data) => {
throw  new  Error('出错了~')
}, (err) => { }).then((data) => {}, (err) => {}).catch((err)=>{
    //错误处理
})
```

-   统一处理错误后，还可以使用then。
    

```
read().then((data) => {
throw  new  Error('出错了~')
}, (err) => { }).then((data) => {}, (err) => {}).catch((err)=>{
    //错误处理
}).then((data) => {}, (err) => {})
```

### Promise.all()

> all方法可以处理多个请求并发的问题。参数是一个数组。all方法调用后会返回一个新的promise。

```
let  fs  =  require('fs');
function  read(url) {
    return  new  Promise((resolve, reject) => {
        fs.readFile(url, 'utf8', function (err, data) {
            if (err) reject();
            resolve(data);
        })
    })
}
Promise.all([read('1.txt'), read('2.txt')]).then((data) => {
    console.log(data)//[ '文本1内容', '文本2内容' ]
}, (err) => {
    console.log(err);
})
```

> 在all方法中一个失败了就全部失败，所以都成功了才会走成功回调。

```
Promise.all([read('1.txt'), read('3.txt')]).then((data) => {
    console.log(data)
}, (err) => {
    console.log('失败了');//失败了
})
```

### Promise.race()

> 多个请求中，谁的返回数据最快，结果就是谁

```
Promise.race([read('1.txt'), read('2.txt')]).then((data) => {
    console.log(data)////文本2内容
}, (err) => {
    console.log('失败了');
})
```

### Promise.resolve()

> 返回一个成功的Promise

```
Promise.resolve('123').then((data) => {
    console.log(data)//123
})
```

### Promise.reject()

> 返回一个失败的Promise

```
Promise.reject('123').then((data) => {
    console.log('err', data)//err 123
})
```