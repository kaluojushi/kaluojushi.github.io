---
title: JavaScript 中 this 的指向问题
date: 2022-04-17 18:57:49
categories: JavaScript
tags:
  - 编程
  - JavaScript
  - 基础
  - 面试
comments: true
---

面试常考题，很多初学者会被 JavaScript 中众多不同情况的 `this` 指向搞迷糊。本质上，`this` 始终指向一个对象。

<!--more-->

## 0 太长不看版

只要弄明白，`this` 在函数中被使用，在定义时不确定指向，**调用时** 才能确定指向（箭头函数除外），并始终 **指向一个对象**，可以简单分为以下 6 种情况：

1. **普通函数调用**，指向全局对象
2. **对象方法调用**，谁调用指向谁，只看调用不看引用
3. **构造函数调用**，指向新构造的实例，尽管可能不被构造函数返回
4. **遇 `apply`、`call`、`bind`**，改变指向
5. **匿名函数调用**，一般指向全局对象
6. **箭头函数调用**，指向定义箭头函数作用域的 `this`

## 1 普通函数调用

普通函数调用时，`this` 指向全局对象，在浏览器中，全局对象为 `window`。看示例：

### 1.1 `var` 定义变量

```javascript
var name = "var";
function fn() {
  var name = "fn-var";
  console.log(this);	// window对象
  console.log(this.name);	// var
}
fn();
```

`fn` 作为普通函数被调用，`this` 指向全局对象 `window`。

`var` 定义的 **全局变量** 视为全局对象的属性，因此 `this.name` 输出全局对象 `window` 的 `name` 属性，即 `"var"`。

### 1.2 `let` 定义变量

```javascript
let name = "let";
function fn() {
  let name = "fn-let";
  console.log(this);	// window对象
  console.log(this.name);	// undefined
}
fn();
```

`let` 定义的变量不会成为全局对象的属性，因此 `this.name` 即全局对象 `window` 的 `name` 属性不存在，输出 `undefined`。

### 1.3 全局对象属性

```javascript
window.name = "win";
function fn() {
  console.log(this);	// window对象
  console.log(this.name);	// win
}
fn();
```

毫无疑问，`this.name` 输出全局对象 `window` 的 `name` 属性，即 `"win"`。

## 2 对象方法调用

函数作为对象方法被调用时，哪个对象调用，`this` 就指向谁。

如果函数变量被赋值给其他变量，依然是谁调用指向谁，而不考虑函数来源 **（只看调用，不看引用）**。

### 2.1 对象直接调用自有方法

```javascript
var obj = {
  age: 15,
  fn() {
    console.log(this.name);	// undefined
    console.log(this.age);	// 15
  }
}
obj.fn();
```

`obj` 对象直接调用自有方法 `fn`，`fn` 中的 `this` 就指向调用它的 `obj` 对象。

### 2.2 对象多层调用自有方法

```javascript
var obj = {
  age: 15,
  inner: {
    age: 20,
    fn() {
      console.log(this.age);
    }
  }
}
obj.inner.fn();	// 20
```

尽管方法 `fn` 是被最外层对象 `obj` 间接调用，但 `fn` 中的 `this` 指向最近的上一层对象 `inner`（即直接调用它的对象）。

### 2.3 函数变量被赋值给其他对象的属性

```javascript
var obj = {
  age: 15,
  fn() {
    console.log(this.name);
    console.log(this.age);
  }
}
var obj2 = {
  name: "obj2",
  age: 20
}
obj2.fn2 = obj.fn;
obj2.fn2();	// obj2 20
```

尽管方法 `fn2` 来源于对象 `obj` 的方法 `fn`，但依然是 **「谁调用指向谁」**，方法被 `obj2` 调用，`this` 就指向 `obj2`。

### 2.4 原型方法

```javascript
var obj = {
  age: 15
}
var obj2 = {
  name: "obj2",
  age: 20
}
Object.prototype.getAge = function () {
  console.log(this.age);
}
obj.getAge();	// 15
obj2.getAge();	// 20
```

这个就很好理解了～虽然 `getAge` 是原型对象 `Object.prototype` 的方法，但实例 `obj` 和 `obj2` 进行了调用，方法中的 `this` 就指向它们。

实际上，这就是原型方法的特点之一：**全体实例的可复用方法**。

### 2.5 函数变量被赋值给普通变量

```javascript
var name = "var";
var age = 123;
var obj = {
  age: 15,
  getName() {
    console.log(this.name);
  }
}
var obj2 = {
  name: "obj2",
  age: 20
}
Object.prototype.getAge = function () {
  console.log(this.age);
}
var fn1 = obj.getName;
let fn2 = obj2.getAge;
fn1();	// var
fn2();	// 123
```

这时，`fn1` 和 `fn2` 实际上是作为普通函数调用，根据 **「谁调用指向谁」** 的原则，它们当中的 `this` 指向全局对象 `window`，输出全局对象的属性值 `"var"` 和 `123`。

总结：无论嵌套几层，无论如何赋值，无论函数来源，谁调用了函数/方法，`this` 就指向谁。

## 3 构造函数调用

一般情况下，`this` 指向构造函数 `new` 出来的对象。

### 3.1 一般情况

```javascript
function A() {
  this.name = "me";
  var age = 15;
  console.log(this.name);	// me
  console.log(this.age);	// undefined
}

var a = new A();
console.log(a);	// A { name: 'me' }
console.log(a.name);	// me
console.log(a.age);	// undefined
```

这也是最常见的过程，类似于定义对象的自有属性。

> 面试常考题：通过 `new` 创建构造函数的一个实例时，发生了什么？
>
> 以 `var a = new A()` 为例：
>
> 1. 创建一个空对象：`var obj = {}`
> 2. 让构造器函数 `A` 的 `this` 指向对象 `obj`，并执行 `A` 中的函数体：`var result = A.call(obj)`
> 3. 设置原型链，让对象 `obj` 的 `__proto__` 属性指向构造器函数 `A` 的原型对象：`obj.__proto__ = A.prototype`
> 4. 判断构造器函数 `A` 的返回类型，如果是值类型，结果返回 `obj`，如果是引用类型（`null` 除外），返回引用类型的对象：`a = result && (typeof result === "object") ? result : obj`
>
> 如果要手写一个 `new` 函数，可能是这样的：
>
> ```javascript
> Function.prototype.new = function () {
>   var obj = {};
>   var result = this.call(obj);
>   obj.__proto__ = this.prototype;
>   return result && (typeof result === "object") ? result : obj;
> }
> ```

### 3.2 构造函数当作普通函数使用

毫无疑问，`this` 指向全局对象 `window`。*这种做法在程序中显然是不推荐的，但做题时要知道 `this` 指向谁。*

```javascript
var name = "var";
var age = 123;

function A() {
  this.name = "me";	// 这里把全局对象的name属性修改了
  var age = 15;
  console.log(this.name);	// me
  console.log(this.age);	// 123
}

A();
```

### 3.3 构造函数返回值的情况

一般情况下，构造函数是不需要显式返回值的，通过 `new` 执行构造函数的返回值是新生成的实例。

但是，如果构造函数返回一个对象（包括空对象），`new` 执行构造函数的返回值不再是新生成的实例，而是该对象。

构造函数执行过程中，`this` 依然试图指向构造函数的实例，虽然这个实例无法返回。

```javascript
function A() {
  this.name = "me";
  console.log(this.name);	// me
  console.log(this.age);	// undefined
  return {
    age: 20
  };
}

var a = new A();
console.log(a);	// { age: 20 }
console.log(a.name);	// undefined
console.log(a.age);	// 20
```

如果构造函数返回的是基本类型、`undefined`、`null` 时，`new` 执行构造函数的返回值依然是新生成的实例。

```javascript
function A() {
  this.name = "me";
  console.log(this.name);	// me
  console.log(this.age);	// undefined
  return null;
}

var a = new A();
console.log(a);	// A { name: 'me' }
console.log(a.name);	// me
console.log(a.age);	// undefined
```

## 4 `apply`、`call`、`bind` 调用

三者均可改变 `this` 的指向给第一个参数。区别在于：

- `apply` 的第二个参数为可迭代对象
- `call` 的参数不固定
- `bind` 除返回一个函数外，与 `call` 并无不同，它需要调用才可以执行，其他二者直接执行函数

```javascript
var obj = {
  age: 15,
  fn(x, y) {
    console.log(this.age + " " + x + " " + y);
  }
}
var obj2 = {
  age: 20
}
obj.fn(1, 2);	// 15 1 2
obj.fn.apply(obj2, [3, 4]);	// 20 3 4
obj.fn.call(obj2, 5, 6);	// 20 5 6
obj.fn.call(obj2, [5, 6]);	// 20 5,6 undefined
obj.fn.bind(obj2, 7, 8)();	// 20 7 8
```

注意以上示例 12、13 行的区别，第 13 行 `[5, 6]` 被当做一个参数传给形参 `x`，形参 `y` 得到 `undefined`。

## 5 匿名函数调用

匿名函数不属于任何对象，它的 `this` 一般指向全局对象。

### 5.1 一般情况

```javascript
var obj = {
  fn() {
    return function () {
      console.log(this);
    }
  },
  inner: {
    fn() {
      return function () {
        console.log(this);
      }
    }
  }
}
obj.fn()();	// window对象
obj.inner.fn()();	// window对象
```

无论谁调用、无论如何调用，这里的匿名函数中的 `this` 指向全局对象 `window`。

### 5.2 定时器回调

定时器回调函数中，`this` 指向全局对象。因此如果需要改变定时器回调函数中 `this` 的指向，需要使用变量提前存储 `this` 的指向。

```javascript
var age = 123;
var obj = {
  age: 15,
  fn() {
    setTimeout(function () {
      console.log(this.age);
    }, 1000);
  }
}
obj.fn();	// 123
```

上述代码中，`this` 指向全局对象 `window`，因此输出全局变量 `age` 的值 `123`。

```javascript
var age = 123;
var obj = {
  age: 15,
  fn() {
    var that = this;
    setTimeout(function () {
      console.log(that.age);
    }, 1000);
  }
}
obj.fn();	// 15
```

上述代码中，使用 `that` 提前存储了 `fn` 中 `this` 的指向，而根据第 2 节的内容，调用 `obj.fn()` 时 `fn` 中的 `this` 指向 `obj`，因此定时器回调函数中能输出 `obj` 的 `age` 属性值 `15`。

### 5.3 事件绑定

注意，事件绑定时，`this` 指向事件源。

```javascript
const btn = document.getElementById("btn");
btn.onclick = function () {
  console.log(this);	// btn对象
}
```

```javascript
const btn = document.getElementById("btn");
btn.addEventListener("click", function () {
  console.log(this);	// btn对象
});
```

如果要清除绑定事件，第一种方法可以通过 `btn.onclick = null` 实现，第二种方法可以调用 `removeEventListener` 方法。

**但是！** 如果下面这么做，移除是失败的。

```javascript
const btn = document.getElementById("btn");
btn.addEventListener("click", function () {
  console.log("点击事件");
});
btn.removeEventListener("click", function () {
  console.log("点击事件");
});
```

创建的两个匿名函数是独立的，彼此没有关系，自然无法被移除。因此，使用 `addEventListener` 绑定事件时，建议传入具名的函数变量，而避免使用匿名函数。

```javascript
const btn = document.getElementById("btn");
btn.addEventListener("click", fn);
btn.removeEventListener("click", fn);

function fn() {
  console.log("点击事件");
}
```

## 6 箭头函数

箭头函数非常特殊，它没有自己的 `this`，而是会寻找 **定义** 箭头函数的作用域中 `this` 的指向。

箭头函数中的 `this` 在其定义时就已确定，与谁调用无关！

```javascript
setTimeout(() => console.log(this), 1000);	// window对象
```

这个没什么问题。

```javascript
var obj = {
  fn() {
    return function () {
      setTimeout(() => console.log(this), 1000);
    }
  }
}
obj.fn()();	// window
```

`this` 会寻找定义箭头函数的作用域（即 `return` 的匿名函数）中 `this` 的指向，因此指向 `window`。

```javascript
var obj = {
  fn() {
    return () => setTimeout(() => console.log(this), 1000);
  }
}
obj.fn()();	// obj
```

`this` 会寻找定义箭头函数的作用域（即 `return` 的箭头函数）中 `this` 的指向，由于外层仍是箭头函数，继续向上寻找，直到找到 `fn` 作用域中的 `this` 且指向为 `obj`。

```javascript
let obj = {
  name: "me"
}
function fn() {
  console.log(this);
  return () => console.log(this);
}
fn()();	// window对象 window对象
let fn2 = fn.call(obj);	// { name: 'me' }
fn2();	// { name: 'me' }
```

- 第 8 行，`fn` 中的两个 `this` 都指向全局对象 `window`，输出两次 `window` 对象。
- 第 9 行，`fn`中的 `this` 被 `call` 改变指向，此时 `this` 指向 `obj`，调用一次 `fn` 函数，输出 `obj` 即 `{ name: 'me' }`
- 第 10 行，`fn2` 是一个箭头函数，在第 9 行中，它的 `this` 被改变指向为 `obj`，因此此处输出 `obj` 即 `{ name: 'me' }`

*为什么第 10 行不是输出 `window` 对象？执行第 10 行时，定义箭头函数的 `fn` 作用域中的 `this` 应该指向 `window` 对象啊。*

箭头函数只看它定义时，当前作用域的 `this` 指向什么。第 6 行，箭头函数是在 `fn` 函数体内被定义的，因此第 9 行执行后，在定义箭头函数时，先是把内部的 `this` 更改为 `obj`，再返回给 `fn2`。这样 `fn2` 调用时（无论如何调用），都输出 `obj` 对象。

## 7 面试题解析

### 7.1 字节跳动面试题

解释以下代码的输出：

```javascript
var name = "bytedance";
function A() {
  console.log(age);
  this.name = 123;
  var age = 2;
  console.log(this.name)
  console.log(this.age);
}
A.prototype.getA = function(){
  console.log(this.name);
  console.log(this);
}
let a = new A();
let funA = a.getA;
funA();
```

输出为：

```
undefined
123
undefined
bytedance
Object [window]
```

- 第 13 行，`new` 一个 `A` 的实例，程序开始执行 `A()`
- 第 3 行，由于第 5 行的变量声明 `var age` 被提升至 **函数顶端**，因此此处 **输出 `undefined`**
- 第 6 行，判断 `this` 指向的是 `a` 实例，此处 **输出定义的 `this.name` 即 123**
- 第 7 行，由于 `this` 指向的 `a` 实例没有 `name` 属性，因此此处 **输出 `undefined`**
- 第 14 行，将 `a.getA` 作为一个函数对象赋值给变量 `funA` 并在第 15 行直接调用，程序开始执行 `getA()`
- 第 10 行，判断 `this` 指向的是全局对象 `window`，因此此处 **输出全局对象的 `name` 属性**，而 `var` 定义的全局变量为全局对象的属性，即输出 `"bytedance"`
- 第 11 行，同上，此处 **输出全局对象 `window`**

**扩展：** 函数对象自带 `name` 属性，且值为本身的名称，如 `A.name` 返回 `"A"`。

## Reference

1. [javascript中this的指向问题 - saucxs - 博客园](https://www.cnblogs.com/chengxs/p/8679313.html)
2. [彻底理解js中this的指向，不必硬背。 - 追梦子 - 博客园](https://www.cnblogs.com/pssp/p/5216085.html)
