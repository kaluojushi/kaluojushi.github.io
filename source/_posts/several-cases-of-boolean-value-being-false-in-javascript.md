---
title: JavaScript中布尔值为false的几种情况
date: 2021-12-13 09:47:52
categories: JavaScript
tags:
  - 编程
  - JavaScript
  - 基础
  - 面试
comments: true
---

前端开发经常需要用到`if`判断，下文归纳JavaScript中布尔值为`false`的几种情况，便于今后开发使用。

<!--more-->

## 1 一共六种情况

1. `undefined`：未定义
2. `null`：空值
3. `false`：布尔值`false`，注意字符串`'false'`为`true`
4. 0：数字0，注意字符串`'0'`为`true`
5. `NaN`：非数值，例如`Math.log(-1)`，注意`typeof NaN`为`number`
6. `""`或`''`：空字符串，注意空格字符串`' '`为`true`

## 2 不同数据类型转换为`false`的情况

| 数据类型  |              转换为`true`               |  转换为`false`   |
| :-------: | :-------------------------------------: | :--------------: |
|  Boolean  |                 `true`                  |     `false`      |
|  String   |             任何非空字符串              | `''`（空字符串） |
|  Number   | 任何非零数值（包括`Infinity`，如`1/0`） |     0和`NaN`     |
|  Object   | 任何对象（包括空数组`[]`、空对象`{}`）  |      `null`      |
| Undefined |                    /                    |   `undefined`    |

**说明：**

1. 使用取反运算符`!`和`!!`可将任何数据类型转换为布尔值，`!!`为本身代表的布尔值，`!`为相反布尔值。

   例如`1-2`为`-1`，`!(1-2)`为`false`，`!!(1-2)`为`true`。

2. 以下是一些数据的求布尔值结果：

   ```javascript
   console.log(false);	// false
   console.log(!!0);	// false
   console.log(!!'');	// false
   console.log(!!null);	// false
   console.log(!!undefined);	// false
   console.log(!!NaN);	// false
   console.log(!!Infinity);	// true
   console.log(!!{});	// true
   console.log(!![]);	//true
   ```

## 3 对空数组和空对象做判断

有时候需要对某个数组或对象做判断，判断其是否为空，可以通过如下方式：

1. 使用`length`属性，适用于数组，如：

   ```javascript
   let array1 = [];
   let array2 = [1, 2];
   if (array1.length) console.log('array1 not empty');
   if (array2.length) console.log('array2 not empty');	// array2 not empty
   ```

2. 使用`for-in`遍历和`hasOwnProperty`判断，并把遍历结果写成函数，适用于数组和对象，如：

   ```javascript
   let array1 = [];
   let array2 = [1, 2];
   let obj1 = {};
   let obj2 = {x: 1, y: 2};
   function isEmpty(obj) {
     for (let key in obj) {
       if (obj.hasOwnProperty(key)) {
         return false;
       }
     }
     return true;
   }
   if (isEmpty(array1)) console.log('array1 is empty');	// array1 is empty
   if (isEmpty(array2)) console.log('array2 is empty');
   if (isEmpty(obj1)) console.log('obj1 is empty');	// obj1 is empty
   if (isEmpty(obj2)) console.log('obj2 is empty');
   ```

## 4 出现`undefined`的几种情况

1. 变量未定义，注意这种情况现在会报错

   ```javascript
   console.log(a);	// ReferenceError: a is not defined
   ```

2. 变量定义后未赋值

   ```javascript
   let x;
   console.log(x);	// undefined
   ```

3. 函数形参未传值

   ```javascript
   function fun(x, y) {
     console.log('x = ' + x);
     console.log('y = ' + y);
   }
   fun(1);
   // x = 1
   // y = undefined
   ```

4. 函数未`return`

   ```javascript
   function fun() {}
   console.log(fun());	// undefined
   ```

5. 函数`return`没有值

   ```javascript
   function fun() {
     return;
   }
   console.log(fun());	// undefined
   ```

6. 对象的属性或方法不存在

   ```javascript
   let obj = {x: 1, y: 2};
   console.log(obj.z);	// undefined
   ```

7. 数组下标越界

   ```javascript
   let array = [1, 2, 3];
   console.log(array[5]);	// undefined
   console.log(array[1.5]);	// undefined
   console.log(array[-1]);	// undefined
   ```

8. 数组`find`方法未找到元素

   ```javascript
   let array = [{id: 1, name: 'a'}, {id: 2, name: 'b'}];
   console.log(array.find(item => item.id === 3));	// undefined
   ```

## 5 出现`null`的几种情况

1. 获取DOM元素时未获取到指定元素对象

   ```html
   <!DOCTYPE html>
   <html lang="en">
   <head>
     <title>Title</title>
   </head>
   <body>
   </body>
   <script>
     let element = document.getElementById('x');
     console.log(element);	// null
   </script>
   </html>
   ```

2. 原型链的顶点，即`Object.prototype.__proto__`

   ```javascript
   console.log(Object.prototype.__proto__);	// null
   ```
   
2. 正则捕获时未捕获到结果，包括：正则对象的`exec`方法未匹配、字符串的`match`方法未匹配。

   ```javascript
   const string = "xyz123ABC2";
   const re1 = /[a-zA-Z]+(\d)/g;
   const re2 = /[a-c]+/g;
   console.log(re1.exec(string));	// [ 'xyz1', '1', index: 0, input: 'xyz123ABC2', groups: undefined ]
   console.log(re2.exec(string));	// null
   console.log(string.match(re1));	// [ 'xyz1', 'ABC2' ]
   console.log(string.match(re2));	// null
   ```

