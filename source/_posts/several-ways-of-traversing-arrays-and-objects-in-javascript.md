---
title: JavaScript中遍历数组、对象的几种方式
date: 2021-12-14 09:33:05
categories: JavaScirpt
tags:
  - 编程
  - JavaScript
  - 基础
  - 面试
comments: true
---

JavaScript中数组和对象均可进行遍历。前端开发时，常会用到对数组、对象以及对象数组进行遍历。

<!--more-->

## 1 普通`for`循环

与其他语言一致。

```javascript
let array = [1, 2, 3, 4, 5];
for (let i = 0; i < array.length; i++) {
  console.log(array[i]);	// 1  2  3  4  5
}
```

可以使用变量缓存数组长度，进行优化。

```javascript
let array = [1, 2, 3, 4, 5];
for (let i = 0, len = array.length; i < len; i++) {
  console.log(array[i]);	// 1  2  3  4  5
}
```

## 2 `for-in`循环

1. 对于数组，`for-in`循环遍历的是数组的**索引值**，例如：

	```javascript
	let array = [1, 3, 7];
	for (const index in array) {
	  console.log(index);	// 0  1  2
	}
	```

	需要注意：这里的索引为**字符串**，而非数值，不能用于计算；且`for-in`会遍历数组所有可枚举属性（包括原型），因此尽量避免使用`for-in`遍历数组。

	```javascript
	let array = [1, 3, 7];
	for (const index in array) {
	  console.log(typeof index);	// string  string  string
	}
	```

2. 对于对象，`for-in`循环遍历的是对象的**键名**，例如：

	```javascript
	let obj = {
	  id: 1,
	  name: 'Carlo'
	};
	for (const key in obj) {
	  console.log(key);	// id  name
	}
	```

	需要注意：`for-in`也会遍历到对象原型链上的方法与属性，可以通过`hasOwnPropery`方法判断属性是否为实例属性。
	
	```javascript
	Object.prototype.school = 'ZJU';	// 原型属性
	let obj = {
	  id: 1,
	  name: 'Carlo'
	};
	for (const key in obj) {
	  if (obj.hasOwnProperty(key)) {
	    console.log(key);	// id  name	不会输出school
	  }
	}
	```

## 3 `for-of`循环

1. 对于数组，`for-of`循环遍历的是数组的**元素值**，例如：

   ```javascript
   let array = [1, 3, 7];
   for (const item of array) {
     console.log(item);	// 1  3  7
   }
   ```

   `for-of`循环不会遍历数组的原型方法与属性，因此是最常用来遍历数组的方法。

2. `for-of`循环**不支持遍历普通对象**，但可以遍历类数组对象、Map和Set等。

以上循环均可以使用`break`、`continue`、`return`语句，例如：

```javascript
let array = [1, 3, 7];
for (const item of array) {
  if (item === 3) continue;
  console.log(item);	// 1  7
}
```

## 4 对象该怎么遍历？

除上文提到的方法外，对象还可以通过以下方式遍历：

1. 键名用`for-in`获取，键值用`obj[key]`获取，会遍历到原型方法与属性。

   ```javascript
   let obj = {
     id: 1,
     name: 'Carlo'
   };
   for (const key in obj) {
     console.log(key + ': ' + obj[key]);	// id: 1  name: Carlo
   }
   ```

   注意：上面第6行取值时**不能写**`obj.key`，这时`key`会被认为是`obj`的一个属性，而非循环变量中的`key`；也就是`obj.key`等同于`obj['key']`。

2. 使用内建的`Object.keys`方法，会获得由对象实例属性组成的数组，不包含原型方法与属性。

   ```javascript
   let obj = {
     id: 1,
     name: 'Carlo'
   };
   for (const key of Object.keys(obj)) {
     console.log(key + ': ' + obj[key]);	// id: 1  name: Carlo
   }
   ```

   或使用下文提到的`forEach`函数：

   ```javascript
   let obj = {
     id: 1,
     name: 'Carlo'
   };
   Object.keys(obj).forEach(key => console.log(key + ': ' + obj[key]));	// id: 1  name: Carlo
   ```

3. 使用内建的`Object.values`方法，会获得由对象实例属性值组成的数组，不包含原型方法与属性；使用内建的`Object.entries`方法，会获得由对象实例属性和属性值组成的数组，不包含原型方法与属性。这两个方法与`Object.keys`类似，在此不表。

3. 类数组对象、字符串、Map、Set等可迭代对象拥有迭代器，可以使用`for-of`遍历。

   ```javascript
   let string = 'hello'
   for (let s of string) {
     console.log(s);	// h  e  l  l  o
   }
   ```

5. 普通对象也可为其添加`Symbol.iterator`方法，并赋一个生成器函数，以使用`for-of`遍历：

   ```javascript
   let obj = {
     id: 1,
     name: 'Carlo',
     [Symbol.iterator]: function* () {
       for (const key of Object.keys(this)) {
         yield this[key];
       }
     }
   };
   for (const value of obj) {
     console.log(value);	// 1  Carlo
   }
   ```

## 5 数组的高阶遍历函数

下文提到的遍历函数均用于数组，在开发中经常使用。

### 5.1 `forEach`函数

对数组进行遍历，参数为一个匿名回调函数，匿名函数的参数为元素值与索引值（可省略），并且此处索引值为数字格式。

```javascript
let array = [1, 3, 7];
array.forEach(function (value, i) {
  console.log(i + ': ' + value);	// 0: 1  1: 3  2: 7
})
```

匿名回调函数可以写成箭头函数，更为精简，下文均使用箭头函数。

```javascript
let array = [1, 3, 7];
array.forEach((value, i) => console.log(i + ': ' + value));
```

`forEach`功能较弱，甚至不如普通`for`循环；并且**不能使用**`break`、`continue`、`return`语句，类似地，其他高阶函数也不能使用（`return`除外）。

### 5.2 `map`函数

`map`是映射的意思，它对原数组的每个元素都遍历一次，同时返回一个新的值，并放到新数组中，因此其支持`return`，但不能用`return`跳出`map`，下同。

例如，我们想把一个数组变成其两倍，可通过`map`实现：

```javascript
let array = [1, 3, 7];
let newArray = array.map(i => i * 2);
console.log(newArray);	// [ 2, 6, 14 ]
```

再例如，对一个对象数组`people`，我们想得到所有人的`id`数组，即`[1, 2, 3]`。

```javascript
let people = [
  {id: 1, name: 'Carlo'},
  {id: 2, name: 'Bill'},
  {id: 3, name: 'Jack'}
];
```

如果使用`forEach`函数，做法如下：

```javascript
let peopleId = [];
people.forEach(person => peopleId.push(person.id));
```

会发现不得不先创造一个新数组。如果使用`map`则精简很多，因为其本身就会返回一个数组：

```javascript
let peopleId = people.map(person => person.id);
```

`map`也常被用在构建二维数组中，如：

```javascript
const dp = new Array(m).fill(0).map(() => new Array(n).fill(0));
```

### 5.3 `filter`函数

`filter`是过滤的意思，它对原数组的每个元素都遍历一次，同时返回判断结果（布尔值），将为`true`的元素放到新数组中。

例如，我们想把一个数组中的奇数提取出来，可通过`filter`实现：

```javascript
let array = [1, 2, 3, 5, 6, 8];
let newArray = array.filter(i => i % 2);	// 数值1会被转换成布尔值true
console.log(newArray);	// [ 1, 3, 5 ]
```

再例如，对对象数组`people`，我们想把名字中第二个字母为a的人提取出来，可通过`filter`实现：

```javascript
let people = [
  {id: 1, name: 'Carlo'},
  {id: 2, name: 'Bill'},
  {id: 3, name: 'Jack'}
];
let somePeople = people.filter(person => person.name.charAt(1) === 'a');
console.log(somePeople);	// [ { id: 1, name: 'Carlo' }, { id: 3, name: 'Jack' } ]
```

### 5.4 `find`函数

`find`是寻找的意思，它对原数组的每个元素都遍历一次，同时返回判断结果（布尔值），将第一个为`true`的元素返回。

例如，对对象数组`people`，我们想把`id`为1的人提取出来，通过`filter`也可实现：

```javascript
let people = [
  {id: 1, name: 'Carlo'},
  {id: 2, name: 'Bill'},
  {id: 3, name: 'Jack'}
];
let somePeople = people.filter(person => person.id === 1);
console.log(somePeople[0]);	// { id: 1, name: 'Carlo' }
```

如果通过`for`循环则是如下逻辑：

```javascript
let people = [
  {id: 1, name: 'Carlo'},
  {id: 2, name: 'Bill'},
  {id: 3, name: 'Jack'}
];
for (let person of people) {
  if (person.id === 1) {
    console.log(person);	// { id: 1, name: 'Carlo' }
    break;
  }
}
```

会发现`for-if-break`是个连续的逻辑，否则会重复遍历。

如果使用`find`则精简很多，因为其本身就会返回所需的元素：

```javascript
let people = [
  {id: 1, name: 'Carlo'},
  {id: 2, name: 'Bill'},
  {id: 3, name: 'Jack'}
];
let person = people.find(person => person.id === 1);
console.log(person);	// { id: 1, name: 'Carlo' }
```

注意：当`filter`找不到任何匹配项时，返回空数组；当`find`找不到任何匹配项时，返回`undefined`。

### 5.5 `reduce`函数

`reduce`可以理解为缩减，它把一个数组逐一缩减为一个值，可通过下面的例子理解。

对下面的对象数组`people`，现在想知道每个人的分数总和，通过`for`循环是这样实现的：

```javascript
let people = [
  {id: 1, name: 'Carlo', points: 90},
  {id: 2, name: 'Bill', points: 95},
  {id: 3, name: 'Jack', points: 88}
];
let sum = 0;
for (let person of people) {
  sum += person.points;
}
console.log(sum);	// 273
```

`reduce`函数传入两个参数，一个是回调函数（参数为初始变量与数组元素），另一个是初始变量的值，即：

```javascript
let people = [
  {id: 1, name: 'Carlo', points: 90},
  {id: 2, name: 'Bill', points: 95},
  {id: 3, name: 'Jack', points: 88}
];
let sum = people.reduce((sum, person) => sum + person.points, 0);
console.log(sum);	// 273
```

逻辑是：先把0赋给`sum`，再对`people`里的每个`person`做遍历，执行回调函数，并将每一步的返回值赋给`sum`。

除累加外，我们还可以使用它进行大小比较。例如，我们想返回分数最高的人，可以这样实现：

```javascript
let people = [
  {id: 1, name: 'Carlo', points: 90},
  {id: 2, name: 'Bill', points: 95},
  {id: 3, name: 'Jack', points: 88}
];
let highest = people.reduce((highest, person) => (highest.points || 0) > person.points ? highest : person, {});
console.log(highest);	// { id: 2, name: 'Bill', points: 95 }
```

如果`reduce`没有给出初始变量的值，数组第一个变量会被赋给初始变量，同时从数组第二个元素开始遍历，注意以下代码的输出结果：

```javascript
let arr = [1, 3, 7, 10];
let sum1 = arr.reduce((sum, n, i) => {
  console.log([sum, n, i]);	// [0,1,0]  [1,3,1]  [4,7,2]  [11,10,3]
  return sum + n;
}, 0);
console.log(sum1);	// 21
let sum2 = arr.reduce((sum, n, i) => {
  console.log([sum, n, i]);	// [1,3,1]  [4,7,2]  [11,10,3]
  return sum + n;
});
console.log(sum2);	// 21
```

### 5.6 结合使用

以上函数可以通过链式编程的思想结合使用。

例如，对一个数组中小于100的数，将其扩大一倍再求和，可以这样实现：

```javascript
let array = [99, 123, 85, 3, 212, 56];
let t = array.filter(i => i < 100).map(i => i * 2).reduce((sum, i) => sum + i, 0);
console.log(t);	// 486
```
