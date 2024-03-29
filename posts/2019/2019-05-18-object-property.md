---
layout: post
title: 如何判断一个对象是否有给定的 Property
date: 2019-05-18
author: aaronchenwei
---

## 两个感叹号(`!!`)

在 js 当中`!!`可以把类型转换成`Boolean`类型的值。

> If it was falsey (e.g. 0, null, undefined, etc.), it will be false, otherwise, true.

上段代码说明一下`!!`的结果。

```javascript
const a = 0;
console.log(!!a); // false

const b = true;
console.log(!!b); // true;

const c = false;
console.log(!!c); // false

const d = [];
console.log(!!d); // true

const e = null;
console.log(!!e); // false

const f = undefined;
console.log(!!f); // false
```

也许我们可以用`!!`来判断一个对象是否有给定的 Property。

```javascript
const demoObject = {
    name: "demo",
    cool: false,
};

console.log(!!demoObject.name); // true (correct result as it's a truthy value)
console.log(!!demoObject.name2); // false

console.log(!!demoObject.cool); // false !!! (not as we expected)
```

可是这个方法最大的问题是`!!`会检查`prototype`链中是否有同名的 Property 存在。如果对象中的 Property 的名字和 prototype 中的一样，这个方法就无法判断了。

```javascript
// Object.prototype.toString
console.log(!!demoObject.toString); // true

// !!Array.prototype.forEach
console.log(!![]["forEach"]); // true
```

**总结**：`!!`在判断一个对象是否有给定的 Property 上面并不太*靠谱*。

## `hasOwnProperty`

`Object.prototype.hasOwnProperty()`可以用来检查一个对象是否有给定的 Property。

> Every object descended from Object inherits the hasOwnProperty method. This method can be used to determine whether an object has the specified property as a direct property of that object; unlike the in operator, this method does not check down the object's prototype chain.

```javascript
const demoObject = {
    name: "Hasky",
    favouriteFood: null,
};

if (demoObject.hasOwnProperty("favouriteFood")) {
    // true
    // do something if it exists
}
```

这个方法也有一个问题。JavaScript 并不保护 Property 的名字。如果对象也有一个 Property 取名叫 hasOwnProperty，这个方法就有一个小问题了。

```javascript
const foo = {
    hasOwnProperty: function () {
        return false;
    },
    bar: "Here be dragons",
};

foo.hasOwnProperty("bar"); // always returns false

// Use another Object's hasOwnProperty
// and call it with 'this' set to foo
({}.hasOwnProperty.call(foo, "bar")); // true

// It's also possible to use the hasOwnProperty property
// from the Object prototype for this purpose
Object.prototype.hasOwnProperty.call(foo, "bar"); // true
```

所以一般在使用 hasOwnProperty，可以自己写个函数。

```javascript
function hasProp(obj, prop) {
    return Object.prototype.hasOwnProperty.call(obj, prop);
}
```

lodash 库中已经有这样的轮子， `has()`，推荐使用。

**总结**：推荐使用`hasOwnProperty`，但不是直接使用。建议使用 lodash 中的`has()`或是自己的封装。

## `prop` `in` myObject

`in`操作符也可以用来判断一个对象是否有给定的 Property。

```javascript
const demoObject = {
    name: "Demo",
    favouriteDrink: null,
    cool: false,
};
"cool" in demoObject; // true
```

`in`的问题也是会查看 prototype 链。所以也不太靠谱。

```javascript
// inherits Object.prototype.toString
"toString" in demoObject; // true
```

**总结**：可以使用，但不是太推荐。

## `typeof`

```javascript
if (typeof demoObject.name !== "undefined") {
    // do something
}
```

**总结**：鉴于`null`, `undefined`的不可靠性，这样的写法可阅读性太差了。不推荐。

## 检测特性(feature)

```javascript
if ("draggable" in document.createElement("div")) {
    // do something if prop exists
}
```
