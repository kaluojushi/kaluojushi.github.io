---
title: 实现一个模板字符串替换，数据源为对象
date: 2022-10-06 00:01:04
categories: JavaScript
tags:
  - 编程
  - JavaScript
  - 基础
  - 面试
  - 正则表达式
comments: true
---

这是当时面阿里云遇到的一个手撕代码题，而且是三道里面最简单的一道……现在看来仍然很有难度。现在一步一步解决它。

<!--more-->

## 1 题目

实现一个`render`函数，函数传入一个字符串和一个对象，将字符串里用`{{ }}`标记的占位符用对象里的属性值替换，并返回新字符串。

示例：

```javascript
let template = "你好，我们是{{company}}，我们来自{{group}}，我们有{{business[0]}}、{{business[1]}}等。"

let obj = {
  company: "阿里",
  group: "蚂蚁",
  business: ["支付宝", "蚂蚁金服", "相互宝"]
}
```

返回：

```
"你好，我们是阿里，我们来自蚂蚁，我们有支付宝、蚂蚁金服等。"
```

题目保证占位符一定是对象里的属性名。

## 2 暴力解法

最简单的方法就是直接使用`split`将字符串分割开，然后使用`switch`循环挨个替换。

```javascript
function render(template, obj) {
  const arr = template.split(/{{|}}/);
  for (let i = 0; i < arr.length; i++) {
    switch (arr[i]) {
      case "company": arr[i] = obj.company; break;
      case "group": arr[i] = obj.group; break;
      case "business[0]": arr[i] = obj.business[0]; break;
      case "business[1]": arr[i] = obj.business[1]; break;
    }
  }
  return arr.join("");
}
```

这样做很不优雅，而且只能解决示例，无法解决其他判例。

## 3 从简单例子开始

既然上面的`split`为了同时分割`{{ }}`使用了正则，那么我们为什么不用正则来做呢！

我们暂时不考虑数组的形式，从一个简单的例子开始：

```javascript
let template = "我叫{{name}}，我今年{{age}}岁了。";

let obj = { name: "小明", age: 18 };

// output: 我叫小明，我今年18岁了。
```

然后来看看几个使用正则的方法：

### 3.1 方法1

```javascript
function render(template, obj) {
  template = template.replace(/{{(\w+)}}/g, "$1");
  Object.keys(obj).forEach(key => {
    template = template.replace(new RegExp(key, 'g'), obj[key]);
  });
  return template;
}
```

这个方法的思路是，先把所有`{{*}}`格式的占位符全部去掉大括号（使用圆括号捕获的`$1`），然后尝试与`obj`的属性名匹配。

这个方法的问题在于：

- 属性名不一定是`\w`（即字母、数字或下划线）。

- 如果出现了不必要替换的字符串，就会出现错误替换，比如：

  ```javascript
  let template = "我的name是{{name}}，我今年{{age}}岁了。";
  ```

因此我们最好把占位符做个整体替换，而非去掉大括号后再替换。

### 3.2 方法2

```javascript
function render(template, obj) {
  const arr = template.match(/{{[a-zA-Z\d]+}}/g);
  for (let i = 0; i < arr.length; i++) {
    arr[i] = arr[i].replace(/{{|}}/g, "");
    template = template.replace("{{" + arr[i] + "}}", obj[arr[i]]);
  }
  return template;
}
```

这个方法是整体替换的思路，先拿出所有`{{*}}`格式的占位符，放到一个数组里，然后通过`replace`去掉大括号，并替换原数组中的占位符。

但是我们发现要多次匹配`{{*}}`，并且属性名也不一定是`[a-zA-Z\d]+`。

### 3.3 方法3

```javascript
function render(template, obj) {
  Object.keys(obj).forEach(key => {
    template = template.replace(`{{${key}}}`, obj[key]);
  });
  return template;
}
```

由于`replace`本身就做了一步正则匹配，遍历所有的键，再匹配替换一步走就行了。

这个方法还是不够精炼，我们希望直接拿到`template`里的占位符`{{*}}`，而不要通过`obj`的`key`。

### 3.4 方法4

```javascript
function render(template, obj) {
  return template.replace(/{{(.*?)}}/g, (match, key) => obj[key]);
}
```

这里补充几个知识：

1. `.*?`是正则固定用法，表示非贪婪匹配，防止从第一个`{{`到最后一个`}}`都被匹配
2. `replace`第二个参数支持一个回调函数，回调函数第一个参数是匹配结果，第二个参数是正则中的子表达式匹配结果（可以有0个或多个该参数），接下来的参数是匹配位置，最后的参数是原串本身

我们用`.*?`匹配了任何`{{*}}`格式的占位符并用圆括号捕获了内部的属性值，然后使用函数接收捕获结果并替换。

## 4 更复杂一点

我们现在打算替换`{{business[0]}}`这样的占位符。这给我们带来了两个问题：

- 如何匹配？可以试试改一下正则表达式
- 如何替换？如果还使用`obj[key]`就不行了，我们必须分开字母与后面的索引

### 4.1 改写方法2

我们改写方法2的正则表达式：

```javascript
function render(template, obj) {
  const arr = template.match(/{{[a-zA-Z\d]+(\[\d+\])?}}/g);
  for (let i = 0; i < arr.length; i++) {
    arr[i] = arr[i].replace(/{{|}}/g, "");
    template = template.replace("{{" + arr[i] + "}}", eval("obj." + arr[i]));
  }
  return template;
}
```

这里使用`?`判断`[*]`表达式存在与否，使用`\[\d+\]`匹配`[*]`表达式。

注意这里使用了`eval`函数，它将传入它的JS字符串作为JS语句执行。这是一个非常危险的方法，我们一般不使用它。

在例子中，`eval`得到的JS语句`obj.business[0]`等同于`obj["business"][0]`，因此得到`"支付宝"`。

### 4.2 改写方法4

方法4的正则表达式无需修改，只要把`eval`加上就行了。

```javascript
function render(template, obj) {
  return template.replace(/{{(.*?)}}/g, (match, key) => eval("obj." + key));
}
```

我们还是不要用`eval`好了，那就在匹配时分别拿到前后两部分。

## 5 不要`eval`

```javascript
function render(template, obj) {
  return template.replace(/{{(.*?)(\[(\d)*\])?}}/g, (match, key, index, num) => index ? obj[key][num] : obj[key]);
}
```

通过这个正则表达式，我们在三个圆括号表达式里分别捕获了属性值、索引和索引内的数字，那就拿到函数里操作就行了。

完美！

## Reference

1. [thymeleaf的遍历拼接字符串_一行代码实现一个简单的模板字符串替换 - 百度文库](https://wenku.baidu.com/view/15486d3f5c0e7cd184254b35eefdc8d376ee14d0.html)
