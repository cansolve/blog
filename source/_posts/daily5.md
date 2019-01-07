---
title: JavaScript中的this
date: 2018-9-22 16:11:23
tags: js
categories: 分享
---

原文链接：[flaviocopes.com](https://flaviocopes.com/javascript-this/)

翻译：我可是兔子

`this`在不同的地方被调用有不同的值。

不知道这些细节可能会导致很多头疼的问题, 所以你不妨花5分钟的时间来了解一下这些坑。

## `this`在严格模式下

除了在声明的对象内被调用,`this`在**严格模式下**永远是`undefined`。

注意我提到的是严格模式。如果不是在严格模式下 (在js的头部，你没有明确的添加`'use strict'`关键字 ), 那么你就处在非严格模式的状态下,`this`在这个环境下， 除了我下面提到的特殊案例外 ，this指代的是全局对象的值。

在浏览器的上下文环境中，这个值就是`window`。

## 在函数方法中的`this`

方法就是以函数形式附属于一个对象。

函数可以有不同的声明形式。

下面就是其中的一种：

```
const car = {
  maker: 'Ford',
  model: 'Fiesta',

  drive() {
    console.log(Driving a ${this.maker} ${this.model} car!)
  }
}

car.drive()
//Driving a Ford Fiesta car!
```

在这个例子中，使用了常规的函数声明的形式,`this`自动绑定为car这个对象。

注意: 上面的函数声明是`drive: function() {`…这样声明的缩写

```
const car = {
  maker: 'Ford',
  model: 'Fiesta',

  drive: function() {
    console.log(Driving a ${this.maker} ${this.model} car!)
  }
}
```

和上面的例子一样的指代:

```
const car = {
  maker: 'Ford',
  model: 'Fiesta'
}

car.drive = function() {
  console.log(Driving a ${this.maker} ${this.model} car!)
}

car.drive()
//Driving a Ford Fiesta car!
```

在同样的语境下，使用箭头函数来声明函数，`this`的指代是不一样的，它属于词法（静态）绑定:

```
const car = {
  maker: 'Ford',
  model: 'Fiesta',

  drive: () => {
    console.log(Driving a ${this.maker} ${this.model} car!)
  }
}

car.drive()
//Driving a undefined undefined car!
```

## 使用箭头函数

你不能在箭头函数中像其他正常的函数声明形式那样给函数绑定一个值来改变this的值。

导致这个的主要原因是箭头函数的工作原理。`this`在箭头函数中是**词法绑定**的, 也就是说它的值仅取决于它在哪个对象下被定义。

## 明确地传递一个对象来改变`this`的值

JavaScript提供了一些方法来映射this所指代的对象，从而得到你想要的值。

在**函数声明**的阶段，使用`bind()`:

```
const car = {
  maker: 'Ford',
  model: 'Fiesta'
}

const drive = function() {
  console.log(Driving a ${this.maker} ${this.model} car!)
}.bind(car)

drive()
//Driving a Ford Fiesta car!
```

你可以绑定一个已定义的对象来改变`this`的值:

```
const car = {
  maker: 'Ford',
  model: 'Fiesta',

  drive() {
    console.log(Driving a ${this.maker} ${this.model} car!)
  }
}

const anotherCar = {
  maker: 'Audi',
  model: 'A4'
}

car.drive.bind(anotherCar)()
//Driving a Audi A4 car!
```

使用`call()`或者`apply()`, 在**函数调用**阶段:

```
const car = {
  maker: 'Ford',
  model: 'Fiesta'
}

const drive = function(kmh) {
  console.log(Driving a ${this.maker} ${this.model} car at ${kmh} km/h!)
}

drive.call(car, 100)
//Driving a Ford Fiesta car at 100 km/h!

drive.apply(car, [100])
//Driving a Ford Fiesta car at 100 km/h!
```

第一个传递给`call()`或者`apply()`的参数是新的`this`所指代的值。 call() 和 apply()两个函数的差异在于第二个参数，apply() 接受的是一个数组来作为它的参数，而call() 函数接受的是一串参数列表。

## DOM事件处理函数的特殊案例

在浏览器事件处理函数的时候,`this`指代的是HTML对象，像下面的这样：

```
document.querySelector('#button').addEventListener('click', function(e) {
  console.log(this) //HTMLElement
}
```

但你可以使用bind() 函数来改变this值：

```
document.querySelector('#button').addEventListener(
  'click',
  function(e) {
    console.log(this) //Window if global, or your context
  }.bind(this)
)
```